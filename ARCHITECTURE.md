# Clawpay System Architecture

**Design target:** 1M+ TPS sustained, 10M+ TPS burst. 99.999% uptime (five 9s = 5.26 min downtime/year). Sub-10ms p99 latency for rule evaluation.

**Design philosophy:** The AI is a build tool, not a runtime dependency. Rules compile to native code. The hot path has zero LLM calls, zero network roundtrips for decisions, zero garbage collection pauses.

---

## First Principles

Before any architecture diagram, three constraints that dictate every decision:

**Constraint 1: Rule evaluation must be O(1) or O(log n), never O(n).**
A bank with 10M accounts running continuous monitoring can't scan every account linearly. Rules compile to decision trees. Lookup, not search.

**Constraint 2: The AI parser runs at BUILD time, never at REQUEST time.**
English → YAML → Compiled Rule happens once, when the developer deploys. At runtime, it's pure deterministic execution. Zero tokens burned per transaction. This is the fundamental architectural difference from Razorpay Agent Studio.

**Constraint 3: Every component must be independently deployable and independently scalable.**
The dispute agent handles 100 events/day. The payment failure agent handles 100K events/day. They don't share a process, a thread pool, or a failure domain.

---

## Core Architecture

```
                    ┌─────────────────────────────────────────┐
                    │           DEVELOPER EXPERIENCE           │
                    │                                          │
                    │  CLI / Dashboard / AI Parser (Build Time) │
                    │  "describe flow" → YAML → Compiled Rule  │
                    └───────────────┬─────────────────────────┘
                                    │ deploy
                    ┌───────────────▼─────────────────────────┐
                    │          CONTROL PLANE                    │
                    │                                          │
                    │  Agent Registry    Rule Compiler          │
                    │  Config Store      Version Manager        │
                    │  RBAC / Auth       Audit Policy           │
                    └───────────────┬─────────────────────────┘
                                    │ distribute compiled rules
                    ┌───────────────▼─────────────────────────┐
                    │            DATA PLANE                     │
                    │                                          │
                    │  ┌──────────┐ ┌──────────┐ ┌──────────┐ │
                    │  │ Agent    │ │ Agent    │ │ Agent    │ │
                    │  │ Cell 1   │ │ Cell 2   │ │ Cell N   │ │
                    │  │          │ │          │ │          │ │
                    │  │ Ingest   │ │ Ingest   │ │ Ingest   │ │
                    │  │ Evaluate │ │ Evaluate │ │ Evaluate │ │
                    │  │ Execute  │ │ Execute  │ │ Execute  │ │
                    │  │ Audit    │ │ Audit    │ │ Audit    │ │
                    │  └──────────┘ └──────────┘ └──────────┘ │
                    └───────────────┬─────────────────────────┘
                                    │
                    ┌───────────────▼─────────────────────────┐
                    │         CASHFREE ACTION LAYER            │
                    │                                          │
                    │  PG  Payouts  Secure ID  Easy Split      │
                    │  RiskShield  Subscriptions  OneEscrow    │
                    └─────────────────────────────────────────┘
```

---

## Layer 1: Developer Experience (Build Time)

This is where AI lives. Not in the hot path.

### AI Parser Pipeline

```
Developer input          AI Parser            Compiler           Registry
     │                      │                    │                  │
     │  "verify PAN,        │                    │                  │
     │   match face,        │                    │                  │
     │   split 85/15"       │                    │                  │
     │─────────────────────>│                    │                  │
     │                      │  YAML config       │                  │
     │                      │───────────────────>│                  │
     │                      │                    │  Compiled        │
     │                      │                    │  decision tree   │
     │                      │                    │─────────────────>│
     │                      │                    │                  │
     │  Review YAML         │                    │                  │
     │<─────────────────────│                    │                  │
     │                      │                    │                  │
     │  Approve (maker)     │                    │                  │
     │─────────────────────────────────────────────────────────────>│
     │                      │                    │                  │
     │  Approve (checker)   │                    │                  │
     │─────────────────────────────────────────────────────────────>│
     │                      │                    │   Deploy to      │
     │                      │                    │   agent cells    │
```

**Key decisions:**
- AI Parser uses Claude/GPT. Called once at config time. Cost: ~$0.02 per flow generated. Amortized over millions of executions = effectively zero.
- YAML is human-readable. Developer reviews every line before deployment.
- Compiler converts YAML rules to a decision tree (similar to how Drools compiles rules to Rete networks, but simpler since our rules are DAG-shaped, not arbitrary).
- Maker-checker enforced at the registry level. No rule goes live without two approvals.

### Rule Compiler

This is the secret weapon. YAML is for humans. The runtime doesn't execute YAML.

```
YAML input:
  rule: cibil_score > 680 AND pan_verified = true AND facematch > 85

Compiles to decision tree:
  Node 0: cibil_score > 680?
    YES → Node 1
    NO  → REJECT (reason: "CIBIL score below threshold")
  Node 1: pan_verified = true?
    YES → Node 2
    NO  → REJECT (reason: "PAN not verified")
  Node 2: facematch > 85?
    YES → APPROVE → trigger action
    NO  → REVIEW (reason: "FaceMatch confidence low")
```

Decision tree evaluation: O(depth). Typical depth: 5-15 nodes. Evaluation time: <1ms.

Compare: LLM evaluation per action (Razorpay): 500ms-2000ms. We're 500-2000x faster on the decision layer.

---

## Layer 2: Control Plane

Manages agent lifecycle. Not in the hot path. Can be slower. Optimized for correctness, not speed.

### Components

**Agent Registry**
- Source of truth for all agent definitions
- Stores: compiled rules, action configs, webhook endpoints, RBAC policies
- Backed by: PostgreSQL (strong consistency) + Redis (cache for hot reads)
- Every agent version is immutable. New version = new entry. Old version = soft delete.
- API: gRPC for internal, REST for external

**Config Store**
- Merchant-specific agent configurations (thresholds, notification preferences, enabled/disabled)
- Backed by: etcd (distributed, strongly consistent, used by Kubernetes itself)
- Config changes propagate to agent cells via watch mechanism (<100ms propagation)

**Version Manager**
- Handles blue-green deployments of rule updates
- Canary: new rule version gets 5% of traffic, then 25%, then 100%
- Rollback: instant (point the pointer back to previous compiled version)

**Audit Policy Engine**
- Defines what gets logged, at what granularity, for how long
- RBI compliance: every rule evaluation + every action execution + every config change
- Retention: configurable per agent type (default: 7 years for financial decisions)

---

## Layer 3: Data Plane (The Hot Path)

This is where the billion-hit architecture lives. Everything here is designed for one thing: evaluate the rule and dispatch the action as fast as physically possible.

### Cell-Based Architecture

**Why cells, not microservices:**

Traditional microservices: every service talks to every other service. One service goes down, cascading failure. Blast radius = entire system.

Cell architecture: each cell is a complete, independent copy of the agent runtime. Cell 1 handles merchants A-M. Cell 2 handles merchants N-Z. Cell 1 goes down? Cell 2 is completely unaffected. Blast radius = one cell.

```
                  Load Balancer (consistent hashing by merchant_id)
                  │                    │                    │
            ┌─────▼─────┐       ┌─────▼─────┐       ┌─────▼─────┐
            │  Cell 1    │       │  Cell 2    │       │  Cell N    │
            │            │       │            │       │            │
            │ ┌────────┐ │       │ ┌────────┐ │       │ ┌────────┐ │
            │ │Ingest  │ │       │ │Ingest  │ │       │ │Ingest  │ │
            │ │Queue   │ │       │ │Queue   │ │       │ │Queue   │ │
            │ └───┬────┘ │       │ └───┬────┘ │       │ └───┬────┘ │
            │ ┌───▼────┐ │       │ ┌───▼────┐ │       │ ┌───▼────┐ │
            │ │Rule    │ │       │ │Rule    │ │       │ │Rule    │ │
            │ │Engine  │ │       │ │Engine  │ │       │ │Engine  │ │
            │ └───┬────┘ │       │ └───┬────┘ │       │ └───┬────┘ │
            │ ┌───▼────┐ │       │ ┌───▼────┐ │       │ ┌───▼────┐ │
            │ │Action  │ │       │ │Action  │ │       │ │Action  │ │
            │ │Dispatch│ │       │ │Dispatch│ │       │ │Dispatch│ │
            │ └───┬────┘ │       │ └───┬────┘ │       │ └───┬────┘ │
            │ ┌───▼────┐ │       │ ┌───▼────┐ │       │ ┌───▼────┐ │
            │ │Audit   │ │       │ │Audit   │ │       │ │Audit   │ │
            │ │Writer  │ │       │ │Writer  │ │       │ │Writer  │ │
            │ └────────┘ │       │ └────────┘ │       │ └────────┘ │
            └────────────┘       └────────────┘       └────────────┘
```

**Cell sizing:**
- Each cell handles ~100K TPS sustained
- For 1M TPS: 10 cells minimum, 15 cells with headroom
- For 10M TPS burst: auto-scale to 100+ cells (Kubernetes HPA)
- Cell boot time: <5 seconds (pre-compiled rules loaded from registry)

### Component Detail: Ingest Queue

**Technology:** NATS JetStream (not Kafka)

Why NATS over Kafka:
- Latency: NATS = sub-millisecond. Kafka = 2-5ms minimum.
- Operational complexity: NATS = single binary. Kafka = ZooKeeper + brokers + schema registry.
- At-least-once delivery with dedup at the rule engine level.
- Cashfree webhooks are the primary event source. NATS handles burst absorption.

```
Webhook → NATS → Rule Engine → Action Dispatch
  1ms      <1ms    <1ms          async
```

**Backpressure handling:**
- NATS JetStream has built-in flow control
- If rule engine is slow: events buffer in NATS (up to configured limit)
- If buffer fills: reject new events with 429 (merchant retries with exponential backoff)
- Never: drop events silently

### Component Detail: Rule Engine

**Technology:** Rust (for the core evaluator) with Go wrapper (for the agent runtime)

Why Rust for the evaluator:
- Zero garbage collection pauses. Java/Go GC pauses can be 10-100ms. For p99 <10ms, GC is the enemy.
- SIMD-optimized comparison operations for batch rule evaluation
- Memory-safe without runtime overhead
- Compiled rules loaded as shared libraries (.so/.dylib)

Why Go for the wrapper:
- Excellent concurrency model (goroutines for managing agent lifecycle)
- Rich ecosystem for Kubernetes, gRPC, observability
- Easier to hire for than Rust

```
Event arrives (JSON)
  │
  ▼
Deserialize to typed struct (zero-copy with FlatBuffers, not JSON parsing)
  │
  ▼
Route to agent (hash lookup by event_type + merchant_id, O(1))
  │
  ▼
Evaluate compiled decision tree (Rust, <1ms for 15-node tree)
  │
  ▼
Decision: APPROVE / REJECT / REVIEW / ESCALATE
  │
  ▼
Dispatch to Action Queue (async, non-blocking)
  │
  ▼
Write to Audit Log (async, non-blocking, guaranteed delivery)
```

**Benchmark targets:**
- Single core: 50K rule evaluations/second
- 16-core machine: 500K evaluations/second (embarrassingly parallel, each evaluation is independent)
- Per cell (4 machines x 16 cores): 2M evaluations/second
- 10 cells: 20M evaluations/second

This is not theoretical. Decision tree evaluation on compiled code is trivially parallelizable. The bottleneck is always I/O (Cashfree API calls), not computation.

### Component Detail: Action Dispatch

**Technology:** Go, async, with circuit breakers

This is the component that calls Cashfree APIs. It's inherently slower because it involves network I/O. Design principle: never let action dispatch slow down rule evaluation.

```
Rule Engine                    Action Dispatch
    │                              │
    │  APPROVE: fire payout        │
    │─────────────────────────────>│  (async, via NATS)
    │                              │
    │  (rule engine continues      │  Call Cashfree Payouts API
    │   processing next event)     │  ← 200ms typical
    │                              │
    │                              │  On success: write audit
    │                              │  On failure: retry with backoff
    │                              │  On 3 failures: dead letter queue
    │                              │  On DLQ: alert ops team
```

**Circuit breaker per Cashfree API:**
- Each API (PG, Payouts, Secure ID) has an independent circuit breaker
- If Payouts API is down: payout actions queue, but KYC actions continue
- Half-open: try one request every 30s. On success, close circuit.
- Uses Hystrix pattern (implemented in Go, not Java Hystrix)

**Rate limiting:**
- Per-merchant rate limits (configurable, default: 100 actions/second)
- Global rate limits per Cashfree API (respect Cashfree's own rate limits)
- Token bucket algorithm (smooth burst handling, not hard cutoff)

**Idempotency:**
- Every action dispatch includes a unique idempotency key (hash of event_id + agent_id + timestamp)
- Cashfree APIs support idempotency headers
- Guarantees exactly-once execution even on retries

### Component Detail: Audit Writer

**Technology:** Event sourcing on ClickHouse (not PostgreSQL)

Why ClickHouse:
- Column-oriented. Optimized for "show me all rule evaluations for merchant X in March" queries.
- Compression: 10-20x vs PostgreSQL for time-series audit data
- Insert throughput: 1M+ rows/second per node
- No row-level locking (append-only, immutable audit trail by design)
- SQL-compatible (compliance teams can query directly)

**Audit schema:**
```sql
CREATE TABLE rule_evaluations (
    event_id        UUID,
    merchant_id     String,
    agent_id        String,
    agent_version   UInt32,
    timestamp       DateTime64(3),
    event_type      LowCardinality(String),
    input_hash      String,         -- SHA256 of input data
    rule_path       Array(String),  -- decision tree path taken
    decision        LowCardinality(String),  -- APPROVE/REJECT/REVIEW
    reason          String,
    action_dispatched String,
    action_status   LowCardinality(String),
    latency_us      UInt32          -- microseconds
) ENGINE = MergeTree()
PARTITION BY toYYYYMM(timestamp)
ORDER BY (merchant_id, agent_id, timestamp)
TTL timestamp + INTERVAL 7 YEAR  -- RBI retention requirement
```

**Immutability guarantee:**
- ClickHouse MergeTree is append-only
- Separate write path (NATS → ClickHouse) from read path (ClickHouse → Dashboard)
- Cryptographic hash chain: each batch includes hash of previous batch (tamper detection)
- Weekly hash checkpoint published to merchant (they can verify their audit trail independently)

---

## Layer 4: Cashfree Action Layer

Pre-built adapters for every Cashfree API. Each adapter handles:
- Authentication (API key management, token refresh)
- Request formatting (per API version)
- Response parsing (normalize to common format)
- Error classification (retryable vs terminal)
- Rate limit tracking

```
Adapter Registry:
  cashfree/pg/create-order          → PG adapter
  cashfree/pg/get-payment           → PG adapter
  cashfree/payouts/transfer         → Payouts adapter
  cashfree/payouts/batch            → Payouts adapter
  cashfree/secure-id/pan-360        → Secure ID adapter
  cashfree/secure-id/facematch      → Secure ID adapter
  cashfree/secure-id/liveness       → Secure ID adapter
  cashfree/easy-split/split         → Easy Split adapter
  cashfree/easy-split/vendor        → Easy Split adapter
  cashfree/subscriptions/mandate    → Subscriptions adapter
  cashfree/disputes/submit-evidence → Disputes adapter
  cashfree/riskshield/score         → RiskShield adapter
  ... (39 products, ~120 adapters)
```

**Adapter interface (Go):**
```go
type CashfreeAdapter interface {
    Execute(ctx context.Context, action Action) (Result, error)
    HealthCheck() error
    RateLimit() RateLimitState
}
```

Every adapter is a plugin. Third-party developers can write adapters for non-Cashfree APIs (Razorpay, Stripe, custom APIs). This is how the open-core model works: the engine is generic, Cashfree adapters are first-class.

---

## Deployment Architecture

### Kubernetes Layout

```
┌─────────────────────────────────────────────────────┐
│  Kubernetes Cluster (EKS / GKE / On-Prem)           │
│                                                     │
│  Namespace: clawpay-control                         │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌──────────┐ │
│  │Registry │ │Compiler │ │Dashboard│ │API Server│ │
│  │(3 pods) │ │(2 pods) │ │(2 pods) │ │(3 pods)  │ │
│  └─────────┘ └─────────┘ └─────────┘ └──────────┘ │
│                                                     │
│  Namespace: clawpay-data                            │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐            │
│  │ Cell 1   │ │ Cell 2   │ │ Cell N   │   ← HPA   │
│  │ (4 pods) │ │ (4 pods) │ │ (4 pods) │            │
│  └──────────┘ └──────────┘ └──────────┘            │
│                                                     │
│  Namespace: clawpay-infra                           │
│  ┌──────┐ ┌──────┐ ┌───────────┐ ┌──────────────┐ │
│  │NATS  │ │Redis │ │ClickHouse │ │ PostgreSQL   │ │
│  │(3)   │ │(3)   │ │(3 shards) │ │ (Primary+RR) │ │
│  └──────┘ └──────┘ └───────────┘ └──────────────┘ │
└─────────────────────────────────────────────────────┘
```

### On-Prem Deployment (Banks)

For banks that need on-prem (V-CIP compliance):

```
Bank Data Center
┌─────────────────────────────────────────┐
│  Docker Compose (minimal deployment)     │
│                                         │
│  clawpay-runtime     (1 container)      │
│  clawpay-nats        (1 container)      │
│  clawpay-clickhouse  (1 container)      │
│  clawpay-postgres    (1 container)      │
│  clawpay-dashboard   (1 container)      │
│                                         │
│  Total: 5 containers, 8GB RAM, 4 vCPU  │
│  Handles: 10K TPS (sufficient for       │
│  most bank branch-level deployments)    │
└─────────────────────────────────────────┘
```

Minimal footprint. No Kubernetes required. `docker-compose up`. Same codebase as cloud, just different scaling.

---

## Reliability Engineering

### Uptime Target: 99.999% (Five 9s)

Five 9s = 5.26 minutes downtime/year. Here's how:

**Failure domain isolation:**
- Cell failure: affects only merchants in that cell. Other cells continue.
- NATS failure: buffered events replay from disk. No data loss.
- ClickHouse failure: audit writes buffer in NATS. Rule evaluation unaffected.
- PostgreSQL failure: config cached in etcd. Agent execution continues with last-known config.
- Cashfree API failure: circuit breaker. Actions queue. Rule evaluation continues.

**Key insight: rule evaluation has ZERO external dependencies at runtime.**
Compiled rules are loaded in memory. Event arrives, rule evaluates, decision made. No database call, no API call, no network roundtrip. The only thing that can stop rule evaluation is the process dying, which is handled by Kubernetes pod restart (<5s).

**What this means practically:**
- Cashfree is down? Rules still evaluate. Actions queue for later.
- Database is down? Rules still evaluate. Audit buffers in NATS.
- NATS is down? Direct in-process evaluation continues. Events buffer in memory.
- Cell crashes? Kubernetes restarts in <5s. Events replay from NATS.

**The only total-system-down scenario:** All cells crash simultaneously AND Kubernetes fails to restart AND NATS loses its disk. This requires a data center failure, not a software failure.

### Observability Stack

```
Metrics:  Prometheus + Grafana
  - Rule evaluations/second (per agent, per cell)
  - p50/p95/p99 latency
  - Action success/failure rates
  - Queue depth (backpressure indicator)
  - Cashfree API response times

Tracing:  OpenTelemetry + Jaeger
  - Full trace: event → rule → action → audit
  - Span for each: deserialization, evaluation, dispatch, ack

Logging:  Structured JSON → Loki
  - Only errors and warnings in production
  - Debug logging via feature flag (per merchant, per agent)

Alerting: Prometheus Alertmanager → PagerDuty/Slack
  - p99 latency > 50ms → warning
  - p99 latency > 200ms → critical
  - Queue depth > 10K → warning
  - Circuit breaker OPEN → critical
  - Cell unavailable > 30s → page oncall
```

---

## Security Architecture

### Bank-Grade by Default

**Data at rest:**
- AES-256 encryption for all stored data (ClickHouse, PostgreSQL)
- Encryption keys in HashiCorp Vault (not in env vars, not in config files)
- Key rotation: automatic, every 90 days

**Data in transit:**
- mTLS between all components (even within the same Kubernetes cluster)
- TLS 1.3 for external APIs
- Certificate management via cert-manager

**Access control:**
- RBAC: Rule Creator, Rule Approver, Rule Deployer, Auditor, Admin
- No single person can create AND approve AND deploy a rule
- Maker-checker enforced at API level (not just UI)
- All access logged with user identity

**Secrets management:**
- Cashfree API keys stored in Vault, injected as ephemeral volumes
- Never in environment variables, never in config maps
- API keys rotated automatically via Vault dynamic secrets

**Network security:**
- Pod security policies: no privileged containers, no host networking
- Network policies: cells can't talk to each other (blast radius containment)
- Egress: only to Cashfree APIs (whitelist, not blacklist)

**Audit trail integrity:**
- Cryptographic hash chain on audit records
- Tamper detection: if any record is modified, the chain breaks
- Independent verification: merchant can verify their audit trail with a hash

---

## Performance Budget

| Operation | Target | How |
|-----------|--------|-----|
| Event ingestion | <1ms | NATS in-process |
| Event deserialization | <0.5ms | FlatBuffers zero-copy |
| Rule evaluation | <1ms | Compiled decision tree (Rust) |
| Action dispatch (async) | <1ms to enqueue | NATS publish |
| Audit write (async) | <1ms to enqueue | NATS publish |
| **Total hot-path latency** | **<5ms p99** | |
| Action execution (Cashfree API) | 100-500ms | Async, not blocking |
| Audit persistence | <100ms | ClickHouse batch insert |

**What the merchant sees:**
- Webhook received → decision made: <5ms
- Webhook received → action executed: 100-500ms (depends on Cashfree API)
- Webhook received → audit searchable: <1 second

---

## Technology Stack Summary

| Component | Technology | Why |
|-----------|-----------|-----|
| Rule evaluator | Rust | Zero GC, SIMD, <1ms evaluation |
| Agent runtime | Go | Concurrency, ecosystem, hiring |
| Event bus | NATS JetStream | Sub-ms latency, simple ops |
| Config store | etcd | Strong consistency, K8s native |
| Metadata DB | PostgreSQL | ACID, proven, tooling |
| Audit store | ClickHouse | Column-store, 1M+ inserts/sec, SQL |
| Cache | Redis Cluster | Hot config, rate limit counters |
| Secrets | HashiCorp Vault | Dynamic secrets, rotation |
| Orchestration | Kubernetes | Cell management, HPA, self-healing |
| Observability | Prometheus + Grafana + Jaeger | Industry standard, OSS |
| CI/CD | GitHub Actions + ArgoCD | GitOps, declarative |
| AI Parser | Claude API (build time) | Best reasoning model, one-time cost |

---

## What This Enables

With this architecture, Clawpay can:

1. **Handle Cashfree's full transaction volume.** Cashfree processes ~6,000 TPS. Clawpay handles 100K+ TPS per cell. One cell is 16x Cashfree's current volume. With 10 cells, it's 160x.

2. **Run continuous monitoring on 100M+ accounts.** Daily batch: 100M accounts x 5 rules each = 500M evaluations. At 2M evaluations/second per cell, one cell finishes in ~250 seconds (~4 minutes). Overnight batch job.

3. **Deploy on-prem in a bank with 5 containers.** Same codebase. Docker Compose. 8GB RAM. 10K TPS. No Kubernetes needed.

4. **Add a new agent in minutes, not months.** Write YAML (or describe in English). Compile. Deploy. Rule evaluation starts in <100ms of deployment.

5. **Survive any single component failure without downtime.** Cell isolation + async everything + in-memory rule evaluation = the hot path has zero external dependencies.

---

## What NOT to Build (Scope Control)

| Don't Build | Use Instead | Why |
|---|---|---|
| Custom message queue | NATS JetStream | Battle-tested, single binary, 10M+ msg/sec |
| Custom database | ClickHouse + PostgreSQL | Don't reinvent storage |
| Custom observability | Prometheus + Grafana + Jaeger | Industry standard, free, extensive ecosystem |
| Custom container orchestration | Kubernetes | Solved problem |
| Custom AI model | Claude API | One-time build-tool usage, not worth training |
| Custom rule language | YAML + compiled decision trees | Developers already know YAML |

**The only custom code:** Rule compiler (YAML → decision tree), Agent runtime (event loop), Cashfree adapters (API wrappers). Everything else is off-the-shelf.

---

## Build Sequence (Engineering Plan)

| Week | Deliverable | Lines of Code (est.) |
|------|-------------|---------------------|
| 1-2 | Rule compiler (YAML → decision tree) | ~3K Rust |
| 3-4 | Agent runtime (event loop + rule evaluator) | ~5K Go |
| 5-6 | 3 Cashfree adapters (PG, Payouts, Secure ID) | ~2K Go |
| 7 | NATS integration + audit writer | ~1.5K Go |
| 8 | Docker Compose + simulation mode | ~1K YAML + Go |
| 9-10 | Dashboard UI (agent builder + audit viewer) | ~4K TypeScript |
| 11-12 | 3 working agents (Dispute Shield, Cart Recovery, Daily Ledger) | ~1K YAML |
| **Total MVP** | **~17.5K lines of code** | **12 weeks, 2 engineers** |

For context: Hyperswitch is ~200K lines of Rust. Clawpay's MVP is 10x smaller because we're not building a payment processor, we're building an agent runtime that calls Cashfree's existing APIs.

---

*The AI is a build tool, not a runtime dependency. Rules compile to native code. The hot path has zero LLM calls. That's the architecture that scales to a billion.*
