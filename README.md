# Clawpay

**AI agents for compliance, lending & money movement.**

The open-core engine that verifies, decides, allocates, and executes — before a single rupee moves. Built on [Cashfree](https://www.cashfree.com) rails.

Inspired by [OpenClaw](https://github.com/nicepkg/openclaw) (250K+ GitHub stars). Positioned as the pre-transaction complement to [Razorpay Agent Studio](https://razorpay.com/blog/agent-studio-ai-agents-by-razorpay/).

## [View the Deck →](https://clawpay.github.io)

---

## The Problem

Razorpay just launched AI agents for **after** money moves — disputes, cart recovery, cashflow forecasting.

Nobody built AI agents for **before** money moves — verify identity, decide eligibility, calculate allocation, execute disbursement, comply with RBI.

Today this is 4–6 vendors, Excel spreadsheets, and 3+ days of human ops.

```
CUSTOMER JOURNEY:

Identity → Onboarding → Verify → Decide → Allocate → Execute
   │                                                      │
   └──────────────── CLAWPAY ─────────────────────────────┘
                                                           │
                                              ↓ money moves ↓
                                                           │
               Disputes → Recovery → Forecast → RTO → Insights
               │                                            │
               └──────── RAZORPAY AGENT STUDIO ────────────┘
```

**Different moments. Same AI-native approach. Together they cover the full lifecycle.**

---

## Three Agents

### 🛡️ Compliance Agent
Pre-configured KYC journeys — CDD, EDD, V-CIP, mule screening. Runs the full verify → decide flow. From 6 vendor integrations to one config.

### 💰 Allocation Agent
Co-lending splits, escrow, TDS, conditional disbursement, time-based release. What finance teams do in Excel today. Fires Cashfree Payouts autonomously.

### 📡 Monitoring Agent
Continuous post-onboarding surveillance. Daily deduplication, dynamic trust scoring, T+1/T+7/T+30 behavioral rules. What KYC Studio can't do.

---

## Quick Start

```bash
docker pull clawpay/gateway
docker-compose up -d
# Open browser → Build flow → Simulate → Deploy
```

### AI Parser — Describe Flows in English

```
"Verify Aadhaar, match face, check CIBIL > 680,
 disburse 80/20 co-lending split"
```

Clawpay generates the YAML and wires it to Cashfree:

```yaml
trigger: webhook/loan-apply
steps:
  - verify: smartocr/aadhaar
  - verify: facematch
  - rule: cibil_score > 680
  - allocate:
      bank_a: 80%
      nbfc_b: 20%
  - execute: cashfree/payouts
```

---

## Why Not Just Use KYC Studio?

We analyzed 14 KYC workflow templates. Honest finding: **~70% work fine with a workflow builder.**

But three use cases genuinely need a rule engine:

| Use Case | Why KYC Studio Can't | Why Clawpay Can |
|----------|---------------------|-----------------|
| **Continuous Monitoring** | Workflow builders run once on trigger. Monitoring needs scheduled autonomous execution — rules firing daily on millions of accounts. | Monitoring Agent runs on cron: daily deduplication, dynamic trust scoring, automated EDD triggers. |
| **Mule Detection (Post-Activation)** | KYC Studio has no concept of "check this account again in 7 days." | T+1, T+7, T+30 behavioral rules. Initial deposit withdrawal patterns. Network graph updates. |
| **Co-Lending Allocation** | No workflow builder handles split math, escrow logic, or conditional disbursement. | Allocation Agent: 80/20 splits, TDS deduction, time-based release, failure routing, reconciliation. |

**Our approach:** Ship the 70% as templates through KYC Studio (immediate win). Build Clawpay for the 30% that genuinely needs it (the defensible product).

---

## Architecture

```
┌──────────────────────────────────────────────────┐
│              CLAWHUB MARKETPLACE                 │
│      Community templates · Cashfree certified    │
└────────────────────┬─────────────────────────────┘
                     │
┌────────────────────▼─────────────────────────────┐
│               CLAWPAY GATEWAY                    │
│                                                  │
│  TRIGGER LAYER    RULE ENGINE    ACTION LAYER    │
│  ────────────     ──────────     ───────────     │
│  Webhooks         Eligibility    PG              │
│  Schedules        Allocation     Payouts         │
│  Form submits     Risk scoring   Verification    │
│  API events       Compliance     SmartOCR        │
│  Doc uploads      Workflow       FaceMatch       │
│                   Retry/Fallback Liveness        │
└────────────────────┬─────────────────────────────┘
                     │
┌────────────────────▼─────────────────────────────┐
│          SIMULATION ENGINE                       │
│  Mock data → Rule preview → A/B test → Audit    │
└────────────────────┬─────────────────────────────┘
                     │
┌────────────────────▼─────────────────────────────┐
│         DOCKER / ON-PREM / CLOUD                 │
│       MIT licensed core · Cashfree powered       │
└──────────────────────────────────────────────────┘
```

### Why Deterministic Rules, Not LLM-Per-Action

Razorpay Agent Studio runs Claude on every action. That works for 50 cart recoveries/day.

For a bank running continuous monitoring on 10M accounts, LLM token costs would be astronomical.

**Clawpay's approach:**
- 99% of decisions → Deterministic rules (zero token cost)
- 1% of decisions → AI-assisted edge cases (justified cost)
- Config generation → AI parser (one-time, high-value)
- Analytics → Natural language querying (on-demand)

---

## Competitive Landscape

| Dimension | Clawpay | Razorpay Agent Studio | HyperVerge | Lentra | Perfios |
|-----------|---------|----------------------|-----------|--------|---------|
| **Focus** | Pre-txn compliance | Post-txn intelligence | Identity verification | Lending platform | Data aggregation |
| **Verify** | ✅ Cashfree native | ❌ None | ✅ World-class | ❌ | Partial |
| **Decide (BRE)** | ✅ Full DAG | LLM reasoning | Linear builder | Lending-only | Bolted-on |
| **Execute** | ✅ PG + Payouts | ✅ PG + Payouts | ❌ | ❌ | ❌ |
| **Allocate** | ✅ Splits, escrow | ❌ | ❌ | ❌ | ❌ |
| **On-Prem** | ✅ Docker, hours | ❌ Cloud-only | Unconfirmed | ❌ Cloud-only | ✅ |
| **Runtime Cost** | Zero tokens | Tokens per action | Per API call | License | License |
| **Open Source** | ✅ MIT core | ❌ | ❌ | ❌ | ❌ |

---

## Market Context

- **$19.3B** Decision Management TAM by 2032 (15.8% CAGR)
- **$133B** India digital lending revenue by 2030
- **62 NBFCs** penalized by RBI (2023–2025) for compliance failures
- **₹250 Cr** maximum penalty under DPDP Act
- **Zero vendors** offer containerized on-prem KYC + BRE + execution stack
- **RBI CLA 2025** (effective Jan 1, 2026): mandatory escrow, 15-day settlement, NPA sync

---

## Roadmap

| Phase | When | What |
|-------|------|------|
| **Compliance Agent MVP** | Q2 2026 | AI parser + YAML spec + simulation + one NBFC design partner |
| **Allocation Agent** | Q3 2026 | Co-lending splits, escrow, TDS + pre-built flow templates |
| **Monitoring Agent + Hub** | Q4 2026 | Continuous monitoring + community template marketplace + open-source core |
| **Enterprise On-Prem** | 2027 | Top 25 banks, ISO 27001, air-gapped Docker, full V-CIP compliance |

---

## Security (Bank-Grade from Day One)

- **RBAC:** Rule creator ≠ approver ≠ deployer
- **Maker-checker:** 4-eyes mandatory on every rule change
- **Immutable audit:** Cryptographically signed execution logs
- **Sandboxed templates:** Community flows never touch production without review
- **Zero data egress:** All processing containerized locally for V-CIP compliance

### Lessons from OpenClaw's Security Crisis
OpenClaw's ClawHub had ~11.9% malicious skills at initial audit. In fintech, a compromised rule redirects money. Every community template requires cryptographic signing, sandbox simulation, and Cashfree security review before deployment.

---

## Pricing

| Tier | Model | For |
|------|-------|-----|
| **Open Core** | Free (MIT) | Developers, startups evaluating |
| **Cloud Managed** | Usage-based per rule execution | Growing NBFCs, fintechs |
| **Enterprise On-Prem** | Annual license + support | Banks, large NBFCs, insurance |

---

## The Deck

Open `index.html` in any browser. Navigate with arrow keys or swipe on mobile. 15 slides.

**Live:** Deploy via GitHub Pages for a shareable URL.

---

## License

Core engine: MIT License
Cashfree integration layer: Requires Cashfree API credentials

---

*Razorpay Agent Studio recovers revenue after the transaction. Clawpay agents ensure compliance before the transaction. Different moments. Same AI-native approach.*
