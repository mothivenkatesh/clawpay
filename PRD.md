# Clawpay: Product Requirements Document

**Version:** 1.0
**Author:** Mothi Venkatesh | PMM, Cashfree Payments
**Date:** March 24, 2026
**Status:** Draft for Review

---

## 1. Overview

### What
Clawpay is an open-source (MIT) agentic payments engine that lets developers define financial workflows as YAML configs. Each workflow is called an "agent." An agent watches for events (webhooks, schedules, API calls), evaluates rules (deterministic decision trees), and fires actions (Cashfree API calls) autonomously.

### Why
Razorpay launched Agent Studio (8 agents, proprietary, post-transaction). The market now understands AI agents on payment rails. But nobody built agents for the pre-transaction and full-lifecycle layer: verify, decide, allocate, disburse, monitor, comply. That's 35 agents across 39 Cashfree products that don't exist today.

### For Whom
- **Primary:** Developers at fintechs, NBFCs, and startups who integrate Cashfree
- **Secondary:** Ops/finance teams at mid-market merchants who configure agents via dashboard
- **Tertiary:** Banks evaluating on-prem deployment for compliance workflows

### Two Layers
- **Clawpay (open-source, MIT):** The engine. Any developer, any payment rail.
- **Cashfree AI Studio (commercial):** Managed product for 8L+ Cashfree merchants. Deeper integration, support, SLA.

---

## 2. Goals and Non-Goals

### Goals
1. Ship a working agent runtime that developers can run locally with `docker-compose up` in under 5 minutes
2. Launch with 3 production-ready agents: Dispute Shield, Cart Recovery, Daily Ledger
3. Achieve 1,000 GitHub stars in first 30 days (signal of developer interest)
4. Enable Cashfree sales team to demo AI Studio to existing merchants within Q2 2026
5. Process first real merchant events (sandbox) within 8 weeks of engineering start

### Non-Goals (Explicit)
1. **No underwriting or credit decisioning.** Cashfree is a PA-PG player under DLG guidelines. No credit scoring, no loan approval, no lending BRE.
2. **No custom AI model training.** AI Parser uses Claude API at build time. We don't train or fine-tune models.
3. **No multi-gateway routing.** FlowWise already does this. Clawpay orchestrates agents, not payment routing.
4. **No mobile SDK.** Web dashboard + API + CLI only for v1.
5. **No real-time streaming analytics.** Audit data is queryable but not streamed to BI tools in v1.

---

## 3. User Personas

### Persona 1: Dev (the builder)
**Profile:** Backend developer at a Series A fintech. Uses Cashfree PG + Payouts. 2-5 years experience. Comfortable with YAML, Docker, APIs.
**Pain:** "I integrated Cashfree PG. Now I need disputes, refunds, settlements, KYC. That's 6 more integrations. Each has different webhook formats, different error codes, different retry logic. I'm spending weeks on plumbing instead of building my product."
**Want:** One runtime, one config format, one event model for all Cashfree products.
**Success metric:** Time from `git clone` to first agent running < 5 minutes.

### Persona 2: Ops (the configurer)
**Profile:** Operations manager at a D2C brand. Not a developer. Uses Cashfree dashboard daily. Manages disputes, refunds, settlements manually.
**Pain:** "I check the Cashfree dashboard every morning. Download settlement reports. Manually match to orders. Chase the finance team for refund approvals. Half my day is dashboard-clicking."
**Want:** Agents that do the dashboard-clicking for them. Daily Ledger via WhatsApp. Auto-dispute response. Cart recovery that runs on its own.
**Success metric:** Hours saved per week > 10.

### Persona 3: CTO (the evaluator)
**Profile:** CTO at a mid-market NBFC. Evaluating build-vs-buy for compliance automation. Reports to a CISO who says no to everything.
**Pain:** "We use HyperVerge for KYC, Lentra for BRE, Cashfree for payouts, Excel for allocation. 4 vendors, 4 dashboards, no single audit trail. RBI audit coming in 3 months."
**Want:** One system that connects verify to decide to execute. On-prem. Audit trail. Maker-checker.
**Success metric:** RBI audit passes without observations on the technology stack.

---

## 4. Core Concepts

### Agent
An agent is a YAML config that defines: trigger (when to run), rules (what to evaluate), actions (what to do), and audit (what to log).

```yaml
agent:
  name: dispute-shield
  version: 1
  trigger:
    type: webhook
    event: cashfree.dispute.created
  rules:
    - if: dispute.amount > 5000 AND dispute.days_remaining < 3
      then: auto_respond
    - if: dispute.amount <= 5000
      then: queue_for_review
  actions:
    auto_respond:
      type: cashfree/disputes/submit-evidence
      params:
        evidence_type: delivery_confirmation
        source: merchant.order_system
    queue_for_review:
      type: notify
      channel: slack
      message: "Dispute ${dispute.id} needs review. ${dispute.days_remaining} days left."
  audit:
    level: full  # log every evaluation + action
    retention: 7y
```

### Event
A JSON payload that triggers an agent. Sources: Cashfree webhooks, scheduled cron, API call, another agent's output.

### Rule
A compiled decision tree. Written as YAML conditions. Compiled to native code at deploy time. Evaluated in <5ms.

### Action
A Cashfree API call (or webhook/notification). Executed async after rule evaluation. Retried on failure. Circuit-breaker protected.

### Audit Record
Immutable log of every rule evaluation. Includes: input data hash, decision path, action dispatched, timestamp, agent version. Stored in ClickHouse. Queryable via SQL.

---

## 5. Swimlane Diagrams

### 5.1 Agent Creation Flow (Developer Experience)

```
Developer          AI Parser (Claude)     Compiler        Registry         Dashboard
    │                    │                   │               │                │
    │  "Describe: auto   │                   │               │                │
    │   respond to       │                   │               │                │
    │   disputes over    │                   │               │                │
    │   5K within 3      │                   │               │                │
    │   days"            │                   │               │                │
    │───────────────────>│                   │               │                │
    │                    │                   │               │                │
    │                    │  Generate YAML    │               │                │
    │                    │  (Claude API,     │               │                │
    │                    │   ~2 sec,         │               │                │
    │                    │   ~$0.02)         │               │                │
    │                    │                   │               │                │
    │  Return YAML       │                   │               │                │
    │<───────────────────│                   │               │                │
    │                    │                   │               │                │
    │  Review YAML       │                   │               │                │
    │  (human verifies   │                   │               │                │
    │   every line)      │                   │               │                │
    │                    │                   │               │                │
    │  Save agent        │                   │               │                │
    │────────────────────────────────────────>│               │                │
    │                    │                   │               │                │
    │                    │                   │  Compile YAML  │                │
    │                    │                   │  to decision   │                │
    │                    │                   │  tree          │                │
    │                    │                   │───────────────>│                │
    │                    │                   │               │                │
    │                    │                   │               │  Store compiled │
    │                    │                   │               │  agent v1      │
    │                    │                   │               │                │
    │                    │                   │               │  Status:       │
    │                    │                   │               │  PENDING       │
    │                    │                   │               │  APPROVAL      │
    │                    │                   │               │                │
    │                    │                   │               │                │
Approver              │                   │               │                │
(Maker-Checker)       │                   │               │                │
    │                    │                   │               │                │
    │  Review + Approve  │                   │               │                │
    │────────────────────────────────────────────────────────>│                │
    │                    │                   │               │                │
    │                    │                   │               │  Status:       │
    │                    │                   │               │  DEPLOYED      │
    │                    │                   │               │                │
    │                    │                   │               │  Push compiled │
    │                    │                   │               │  rule to       │
    │                    │                   │               │  runtime cells │
    │                    │                   │               │                │
    │                    │                   │               │  Agent live.   │
    │                    │                   │               │  <100ms.       │
    │                    │                   │               │                │
```

**Key points:**
- AI Parser is optional. Developer can write YAML directly.
- Human reviews YAML before save. AI doesn't deploy anything autonomously.
- Compiler converts YAML to decision tree. This is the build step.
- Maker-checker: creator cannot approve their own agent. Requires second person.
- Deploy is instant (<100ms). Compiled rules pushed to running cells.

---

### 5.2 Agent Runtime Flow (Event Processing)

```
Cashfree           NATS            Agent Cell          Cashfree API       ClickHouse
Webhook            (Event Bus)     (Runtime)           (Action Target)    (Audit)
    │                  │               │                    │                │
    │  POST /webhook   │               │                    │                │
    │  dispute.created │               │                    │                │
    │  {amount: 8000,  │               │                    │                │
    │   days_left: 2}  │               │                    │                │
    │─────────────────>│               │                    │                │
    │                  │               │                    │                │
    │  200 OK (ack)    │  Publish to   │                    │                │
    │<─────────────────│  agent queue  │                    │                │
    │                  │──────────────>│                    │                │
    │  (<1ms total)    │               │                    │                │
    │                  │               │                    │                │
    │                  │               │  STEP 1: Deserialize              │
    │                  │               │  event (FlatBuffers, <0.5ms)      │
    │                  │               │                    │                │
    │                  │               │  STEP 2: Route to agent           │
    │                  │               │  (hash lookup, O(1))              │
    │                  │               │                    │                │
    │                  │               │  STEP 3: Evaluate rules           │
    │                  │               │  dispute.amount > 5000? YES       │
    │                  │               │  dispute.days_left < 3? YES       │
    │                  │               │  Decision: AUTO_RESPOND           │
    │                  │               │  (<1ms)            │                │
    │                  │               │                    │                │
    │                  │               │  STEP 4: Dispatch action (async)  │
    │                  │               │─────────────────────>│              │
    │                  │               │                    │                │
    │                  │               │  STEP 5: Write audit (async)      │
    │                  │               │──────────────────────────────────>│
    │                  │               │                    │                │
    │                  │               │  (Steps 4+5 are    │                │
    │                  │               │   non-blocking.    │                │
    │                  │               │   Runtime continues│                │
    │                  │               │   processing next  │                │
    │                  │               │   event immediately)│               │
    │                  │               │                    │                │
    │                  │               │                    │  Submit        │
    │                  │               │                    │  evidence      │
    │                  │               │                    │  to dispute    │
    │                  │               │                    │                │
    │                  │               │                    │  200 OK        │
    │                  │               │                    │───────>│       │
    │                  │               │                    │        │       │
    │                  │               │  Action result     │        │       │
    │                  │               │<─────────────────────       │       │
    │                  │               │                    │        │       │
    │                  │               │  Update audit with │        │       │
    │                  │               │  action result     │        │       │
    │                  │               │───────────────────────────>│       │
    │                  │               │                    │        │       │
    │                  │               │                    │ AUDIT RECORD:  │
    │                  │               │                    │ event_id: xxx  │
    │                  │               │                    │ agent: dispute │
    │                  │               │                    │ decision: AUTO │
    │                  │               │                    │ action: submit │
    │                  │               │                    │ status: OK     │
    │                  │               │                    │ latency: 203ms │
    │                  │               │                    │                │
```

**Latency breakdown:**
- Webhook receive to ack: <1ms (NATS publish + 200 OK)
- Event to decision: <3ms (deserialize + route + evaluate)
- Decision to action dispatched: <1ms (NATS publish, async)
- Action execution: 100-500ms (Cashfree API, async)
- Total hot-path: <5ms. Merchant webhook response: <1ms.

---

### 5.3 Dispute Shield Agent (End-to-End Example)

```
Customer        Card Network     Cashfree         Clawpay           Merchant         Cashfree
(cardholder)    (Visa/MC)        (Disputes)       (Dispute Shield)  (Order System)   (Disputes API)
    │               │                │                │                │                │
    │  "I didn't    │                │                │                │                │
    │   authorize   │                │                │                │                │
    │   this"       │                │                │                │                │
    │──────────────>│                │                │                │                │
    │               │                │                │                │                │
    │               │  Chargeback    │                │                │                │
    │               │  notification  │                │                │                │
    │               │───────────────>│                │                │                │
    │               │                │                │                │                │
    │               │                │  Webhook:      │                │                │
    │               │                │  dispute.      │                │                │
    │               │                │  created       │                │                │
    │               │                │───────────────>│                │                │
    │               │                │                │                │                │
    │               │                │                │  RULE 1:       │                │
    │               │                │                │  amount > 5K   │                │
    │               │                │                │  AND days < 3? │                │
    │               │                │                │  = YES         │                │
    │               │                │                │                │                │
    │               │                │                │  ACTION:       │                │
    │               │                │                │  Gather        │                │
    │               │                │                │  evidence      │                │
    │               │                │                │                │                │
    │               │                │                │  Pull order    │                │
    │               │                │                │───────────────>│                │
    │               │                │                │                │                │
    │               │                │                │  Order #4521   │                │
    │               │                │                │  Shipped: Y    │                │
    │               │                │                │  Tracking: Y   │                │
    │               │                │                │  Delivered: Y  │                │
    │               │                │                │<───────────────│                │
    │               │                │                │                │                │
    │               │                │                │  Pull Secure   │                │
    │               │                │                │  ID data (if   │                │
    │               │                │                │  KYC was done) │                │
    │               │                │                │                │                │
    │               │                │                │  Compile       │                │
    │               │                │                │  evidence:     │                │
    │               │                │                │  - Order data  │                │
    │               │                │                │  - Delivery    │                │
    │               │                │                │    proof       │                │
    │               │                │                │  - IP/device   │                │
    │               │                │                │  - KYC match   │                │
    │               │                │                │    (if avail)  │                │
    │               │                │                │                │                │
    │               │                │                │  Submit via    │                │
    │               │                │                │  Disputes API  │                │
    │               │                │                │───────────────────────────────>│
    │               │                │                │                │                │
    │               │                │                │                │  Evidence      │
    │               │                │                │                │  accepted      │
    │               │                │                │<───────────────────────────────│
    │               │                │                │                │                │
    │               │                │                │  AUDIT:        │                │
    │               │                │                │  Dispute #789  │                │
    │               │                │                │  Auto-responded│                │
    │               │                │                │  Within 4 hrs  │                │
    │               │                │                │  Evidence: 4   │                │
    │               │                │                │  items         │                │
    │               │                │                │                │                │
    │               │                │                │  NOTIFY:       │                │
    │               │                │                │  Slack/WhatsApp│                │
    │               │                │                │  "Dispute #789 │                │
    │               │                │                │   auto-resolved│                │
    │               │                │                │   Evidence     │                │
    │               │                │                │   submitted."  │                │
    │               │                │                │                │                │
```

**Without Clawpay:** Ops team sees dispute on day 6. Scrambles. Submits screenshot. Loses.
**With Clawpay:** Evidence auto-compiled and submitted within 4 hours. Win rate: 20% to 50%.

---

### 5.4 Cart Recovery Agent (End-to-End Example)

```
Customer         Merchant         Clawpay            Timer           WhatsApp         Cashfree
(shopper)        (Website)        (Cart Reclaimer)   (30 min wait)   (Notification)   (Payment Links)
    │                │                │                │                │                │
    │  Add to cart   │                │                │                │                │
    │  ₹4,500        │                │                │                │                │
    │───────────────>│                │                │                │                │
    │                │                │                │                │                │
    │  Begin checkout│                │                │                │                │
    │───────────────>│                │                │                │                │
    │                │                │                │                │                │
    │  ...leaves     │                │                │                │                │
    │  without paying│                │                │                │                │
    │                │                │                │                │                │
    │                │  Event:        │                │                │                │
    │                │  checkout.     │                │                │                │
    │                │  abandoned     │                │                │                │
    │                │  {cart: ₹4500, │                │                │                │
    │                │   phone: xxx}  │                │                │                │
    │                │───────────────>│                │                │                │
    │                │                │                │                │                │
    │                │                │  RULE:         │                │                │
    │                │                │  cart > ₹500?  │                │                │
    │                │                │  = YES         │                │                │
    │                │                │                │                │                │
    │                │                │  ACTION:       │                │                │
    │                │                │  Schedule       │                │                │
    │                │                │  recovery      │                │                │
    │                │                │  (30 min delay)│                │                │
    │                │                │───────────────>│                │                │
    │                │                │                │                │                │
    │                │                │                │  30 min later  │                │
    │                │                │                │                │                │
    │                │                │  Timer fires   │                │                │
    │                │                │<───────────────│                │                │
    │                │                │                │                │                │
    │                │                │  Check: did     │                │                │
    │                │                │  customer pay   │                │                │
    │                │                │  in the         │                │                │
    │                │                │  meantime?      │                │                │
    │                │                │                │                │                │
    │                │                │  = NO           │                │                │
    │                │                │                │                │                │
    │                │                │  Create Payment │                │                │
    │                │                │  Link with      │                │                │
    │                │                │  ₹200 discount  │                │                │
    │                │                │──────────────────────────────────────────────────>│
    │                │                │                │                │                │
    │                │                │                │                │  Link created  │
    │                │                │                │                │  ₹4,300        │
    │                │                │<──────────────────────────────────────────────────│
    │                │                │                │                │                │
    │                │                │  Send WhatsApp  │                │                │
    │                │                │──────────────────────────────────>│               │
    │                │                │                │                │                │
    │  WhatsApp:     │                │                │                │                │
    │  "Your cart    │                │                │                │                │
    │   is waiting!  │                │                │                │                │
    │   ₹200 off.    │                │                │                │                │
    │   Pay ₹4,300   │                │                │                │                │
    │   [Pay Now]"   │                │                │                │                │
    │<───────────────────────────────────────────────────│               │                │
    │                │                │                │                │                │
    │  Clicks link   │                │                │                │                │
    │  Pays ₹4,300   │                │                │                │                │
    │──────────────────────────────────────────────────────────────────>│                │
    │                │                │                │                │                │
    │                │                │  AUDIT:        │                │                │
    │                │                │  Cart #1234    │                │                │
    │                │                │  Recovered     │                │                │
    │                │                │  Revenue:₹4300 │                │                │
    │                │                │  Discount:₹200 │                │                │
    │                │                │  ROI: 21.5x    │                │                │
    │                │                │                │                │                │
```

**Without Clawpay:** Cart abandoned. Customer gone. Revenue lost.
**With Clawpay:** 5-10% recovery rate. On ₹1Cr/month: ₹5-10L recovered. Cost: ₹0.50 per WhatsApp message.

---

### 5.5 Marketplace Split Agent (End-to-End Example)

```
Buyer            Merchant          Cashfree PG       Clawpay            Easy Split       Vendor A        Vendor B
                 (Marketplace)                       (Split Agent)                       (₹3,400)        (₹1,200)
    │                │                │                │                │                │                │
    │  Pay ₹5,000    │                │                │                │                │                │
    │  (2 vendors)   │                │                │                │                │                │
    │───────────────>│                │                │                │                │                │
    │                │  Create order  │                │                │                │                │
    │                │───────────────>│                │                │                │                │
    │                │                │  Payment       │                │                │                │
    │                │                │  successful    │                │                │                │
    │                │                │  ₹5,000        │                │                │                │
    │                │                │───────────────>│                │                │                │
    │                │                │                │                │                │                │
    │                │                │                │  RULE:         │                │                │
    │                │                │                │  Vendor A:     │                │                │
    │                │                │                │   items: ₹4000 │                │                │
    │                │                │                │   commission:  │                │                │
    │                │                │                │   15% = ₹600   │                │                │
    │                │                │                │   TDS 1% = ₹40 │                │                │
    │                │                │                │   Net: ₹3,360  │                │                │
    │                │                │                │                │                │                │
    │                │                │                │  Vendor B:     │                │                │
    │                │                │                │   items: ₹1000 │                │                │
    │                │                │                │   commission:  │                │                │
    │                │                │                │   15% = ₹150   │                │                │
    │                │                │                │   TDS 1% = ₹10 │                │                │
    │                │                │                │   Net: ₹840    │                │                │
    │                │                │                │                │                │                │
    │                │                │                │  Platform:     │                │                │
    │                │                │                │   Commission:  │                │                │
    │                │                │                │   ₹750         │                │                │
    │                │                │                │   TDS held:    │                │                │
    │                │                │                │   ₹50          │                │                │
    │                │                │                │                │                │                │
    │                │                │                │  Split via     │                │                │
    │                │                │                │  Easy Split    │                │                │
    │                │                │                │───────────────>│                │                │
    │                │                │                │                │                │                │
    │                │                │                │                │  ₹3,360 to     │                │
    │                │                │                │                │  Vendor A      │                │
    │                │                │                │                │  (settlement   │                │
    │                │                │                │                │   per schedule)│                │
    │                │                │                │                │───────────────>│                │
    │                │                │                │                │                │                │
    │                │                │                │                │  ₹840 to       │                │
    │                │                │                │                │  Vendor B      │                │
    │                │                │                │                │───────────────────────────────>│
    │                │                │                │                │                │                │
    │                │                │                │  NOTIFY:       │                │                │
    │                │                │                │  WhatsApp to   │                │                │
    │                │                │                │  each vendor:  │                │                │
    │                │                │                │  "Order #4521  │                │                │
    │                │                │                │   ₹4,000 sale  │                │                │
    │                │                │                │   ₹600 comm    │                │                │
    │                │                │                │   ₹40 TDS      │                │                │
    │                │                │                │   ₹3,360 net   │                │                │
    │                │                │                │   settling T+2"│                │                │
    │                │                │                │                │                │                │
    │                │                │                │  AUDIT:        │                │                │
    │                │                │                │  Split #789    │                │                │
    │                │                │                │  2 vendors     │                │                │
    │                │                │                │  ₹750 platform │                │                │
    │                │                │                │  ₹50 TDS held  │                │                │
    │                │                │                │  Reconciled: Y │                │                │
    │                │                │                │                │                │                │
```

**Without Clawpay:** Finance team calculates in Excel. Manual payout. Reconciliation takes days. TDS errors.
**With Clawpay:** Auto-calculated. Auto-split. Vendor notified. Reconciliation instant. TDS accurate.

---

### 5.6 KYC Onboarding Agent (End-to-End Example)

```
Customer         Merchant App      Clawpay             Secure ID          Secure ID        Secure ID
(new user)                         (KYC Agent)         (PAN 360)          (DigiLocker)     (FaceMatch)
    │                │                │                    │                │                │
    │  Sign up       │                │                    │                │                │
    │  Phone: 98xxx  │                │                    │                │                │
    │───────────────>│                │                    │                │                │
    │                │  Event:        │                    │                │                │
    │                │  user.signup   │                    │                │                │
    │                │  {phone,name}  │                    │                │                │
    │                │───────────────>│                    │                │                │
    │                │                │                    │                │                │
    │                │                │  STEP 1: PAN       │                │                │
    │                │                │  verification      │                │                │
    │                │                │───────────────────>│                │                │
    │                │                │                    │                │                │
    │                │                │  PAN valid.        │                │                │
    │                │                │  Name match: 94%   │                │                │
    │                │                │<───────────────────│                │                │
    │                │                │                    │                │                │
    │                │                │  RULE: name_match  │                │                │
    │                │                │  > 80%? YES        │                │                │
    │                │                │  Continue.         │                │                │
    │                │                │                    │                │                │
    │                │                │  STEP 2: Aadhaar   │                │                │
    │                │                │  via DigiLocker    │                │                │
    │                │                │──────────────────────────────────>│                │
    │                │                │                    │                │                │
    │  Customer      │                │                    │                │                │
    │  approves      │                │                    │                │                │
    │  DigiLocker    │                │                    │                │                │
    │  consent       │                │                    │                │                │
    │                │                │  Aadhaar verified  │                │                │
    │                │                │<──────────────────────────────────│                │
    │                │                │                    │                │                │
    │                │                │  STEP 3: FaceMatch │                │                │
    │                │                │  (selfie vs        │                │                │
    │                │                │   Aadhaar photo)   │                │                │
    │                │                │──────────────────────────────────────────────────>│
    │                │                │                    │                │                │
    │                │                │  FaceMatch: 91%    │                │                │
    │                │                │  Liveness: PASS    │                │                │
    │                │                │<──────────────────────────────────────────────────│
    │                │                │                    │                │                │
    │                │                │  RULE: ALL passed? │                │                │
    │                │                │  PAN: YES          │                │                │
    │                │                │  Aadhaar: YES      │                │                │
    │                │                │  FaceMatch > 85%:  │                │                │
    │                │                │  YES               │                │                │
    │                │                │                    │                │                │
    │                │                │  DECISION:         │                │                │
    │                │                │  AUTO_APPROVE      │                │                │
    │                │                │                    │                │                │
    │                │  Callback:     │                    │                │                │
    │                │  user.verified │                    │                │                │
    │                │  status: OK    │                    │                │                │
    │                │<───────────────│                    │                │                │
    │                │                │                    │                │                │
    │  "Welcome!     │                │  AUDIT:            │                │                │
    │   Account      │                │  KYC #456          │                │                │
    │   activated."  │                │  PAN: OK (94%)     │                │                │
    │<───────────────│                │  Aadhaar: OK       │                │                │
    │                │                │  Face: OK (91%)    │                │                │
    │                │                │  Decision: APPROVE │                │                │
    │                │                │  Time: 28 seconds  │                │                │
    │                │                │                    │                │                │
```

**Without Clawpay:** 6 API integrations. 3-5 day onboarding. 40% drop-off.
**With Clawpay:** 1 YAML config. 28-second onboarding. Single audit trail.

---

### 5.7 Scheduled Agent Flow (Daily Ledger, Continuous Monitoring)

```
Cron Scheduler     Clawpay            Cashfree           ClickHouse       WhatsApp
(8am daily)        (Daily Ledger)     (Settlements API)  (Query)          (Notification)
    │                │                    │                │                │
    │  8:00 AM       │                    │                │                │
    │  trigger       │                    │                │                │
    │───────────────>│                    │                │                │
    │                │                    │                │                │
    │                │  Fetch yesterday's │                │                │
    │                │  settlements       │                │                │
    │                │───────────────────>│                │                │
    │                │                    │                │                │
    │                │  Settlements data  │                │                │
    │                │  Gross: ₹8.4L     │                │                │
    │                │  Fees: ₹12K       │                │                │
    │                │  Net: ₹8.28L      │                │                │
    │                │<───────────────────│                │                │
    │                │                    │                │                │
    │                │  Query pending     │                │                │
    │                │  disputes          │                │                │
    │                │───────────────────────────────────>│                │
    │                │                    │                │                │
    │                │  3 open disputes   │                │                │
    │                │  ₹45K held        │                │                │
    │                │<───────────────────────────────────│                │
    │                │                    │                │                │
    │                │  Format message    │                │                │
    │                │                    │                │                │
    │                │  Send via WhatsApp │                │                │
    │                │──────────────────────────────────────────────────>│
    │                │                    │                │                │
    │                │                    │                │  Merchant gets:│
    │                │                    │                │                │
    │                │                    │                │  "Good morning!│
    │                │                    │                │   Yesterday:   │
    │                │                    │                │   Collected:   │
    │                │                    │                │   ₹8.4L       │
    │                │                    │                │   Fees: ₹12K  │
    │                │                    │                │   Settled:     │
    │                │                    │                │   ₹8.28L      │
    │                │                    │                │   Pending T+2: │
    │                │                    │                │   ₹2.1L       │
    │                │                    │                │   Disputes: 3  │
    │                │                    │                │   (₹45K held)  │
    │                │                    │                │                │
    │                │                    │                │   No login     │
    │                │                    │                │   needed."     │
    │                │                    │                │                │
```

**Without Clawpay:** Merchant opens dashboard every morning. 30 min checking numbers.
**With Clawpay:** WhatsApp at 8am. 10-second glance. Done.

---

## 6. Agent YAML Specification

### Full Schema

```yaml
agent:
  name: string              # unique identifier (kebab-case)
  version: integer           # auto-incremented on save
  description: string        # human-readable purpose
  enabled: boolean           # can be disabled without deletion

  trigger:
    type: webhook | schedule | api | chain
    # webhook: Cashfree webhook event
    event: string            # e.g., "cashfree.dispute.created"
    # schedule: cron expression
    cron: string             # e.g., "0 8 * * *" (8am daily)
    # chain: output of another agent
    from_agent: string       # agent name
    on_decision: string      # e.g., "APPROVE"

  inputs:
    - name: string
      source: trigger | cashfree_api | merchant_api | static
      api: string            # if source is cashfree_api or merchant_api
      cache_ttl: duration    # e.g., "5m", "1h", "0" for no cache

  rules:
    # Simple condition
    - if: expression         # e.g., "amount > 5000 AND days_left < 3"
      then: action_name

    # Chained conditions (decision tree)
    - if: expression
      then: action_name
      else_if: expression
      else_then: action_name
      else: action_name

    # Threshold with fallback
    - if: score > 85
      then: auto_approve
    - if: score > 50 AND score <= 85
      then: manual_review
    - default: reject

  actions:
    action_name:
      type: cashfree_api | webhook | notify | chain

      # cashfree_api: calls a Cashfree endpoint
      api: string            # e.g., "cashfree/disputes/submit-evidence"
      params: object         # API-specific parameters

      # webhook: calls merchant's endpoint
      url: string
      method: GET | POST
      headers: object
      body: template         # supports ${variable} interpolation

      # notify: sends notification
      channel: slack | whatsapp | email
      message: template

      # chain: triggers another agent
      agent: string
      with: object           # pass data to chained agent

    # Retry configuration (per action)
    retry:
      max_attempts: integer  # default: 3
      backoff: linear | exponential
      interval: duration     # base interval, e.g., "30s"

    # Circuit breaker (per action)
    circuit_breaker:
      failure_threshold: integer  # default: 5
      recovery_timeout: duration  # default: "30s"

  audit:
    level: full | decision_only | off
    retention: duration      # e.g., "7y" for RBI compliance
    include_input: boolean   # whether to log full input data (PII consideration)
    hash_pii: boolean        # SHA256 PII fields before logging

  notifications:
    on_error:
      channel: slack | email
      target: string         # channel name or email
    on_circuit_open:
      channel: slack
      target: string
```

### Expression Language (Rules)

Simple, deterministic, no Turing-completeness (intentional).

```
# Comparison
amount > 5000
score >= 85
status == "active"
days_remaining < 3

# Logical
amount > 5000 AND days_remaining < 3
score < 50 OR blacklist_match == true
NOT (status == "verified")

# String matching
email CONTAINS "@gmail.com"
name STARTS_WITH "test"
pincode IN ["110001", "110002", "110003"]

# Null/existence checks
tracking_number EXISTS
refund_id IS_NULL

# Arithmetic (in conditions only, not assignment)
(amount * 0.15) > 1000
```

**Intentionally NOT supported:**
- Loops (no `for`, `while`)
- Variables/assignment (no `x = 5`)
- Function calls (no `calculateRisk()`)
- External data fetching (data is pre-fetched in `inputs` section)

Why: A Turing-complete rule language is a BRE. We're building an agent config, not a programming language. If you need complex logic, write a Go plugin. Keep the YAML simple.

---

## 7. API Specification (Runtime)

### Agent Management

```
POST   /api/v1/agents              Create agent (YAML body)
GET    /api/v1/agents              List all agents
GET    /api/v1/agents/:name        Get agent by name
PUT    /api/v1/agents/:name        Update agent (new version created)
DELETE /api/v1/agents/:name        Disable agent (soft delete)
POST   /api/v1/agents/:name/deploy Deploy specific version
POST   /api/v1/agents/:name/rollback Rollback to previous version
```

### Event Ingestion

```
POST   /api/v1/events              Submit event (triggers matching agents)
POST   /api/v1/webhooks/cashfree   Cashfree webhook receiver
```

### Simulation

```
POST   /api/v1/simulate            Simulate event through agent (no real actions)
GET    /api/v1/simulate/:id        Get simulation result
```

### Audit

```
GET    /api/v1/audit                Query audit records (ClickHouse SQL)
GET    /api/v1/audit/:event_id     Get full audit trail for an event
GET    /api/v1/audit/agent/:name   Get all evaluations for an agent
```

### AI Parser

```
POST   /api/v1/parse               Natural language to YAML (Claude API)
```

### Health

```
GET    /health                     Liveness check
GET    /ready                      Readiness check (all dependencies up)
GET    /metrics                    Prometheus metrics
```

---

## 8. Dashboard UI (v1 Scope)

### Screens

| Screen | Purpose | Priority |
|---|---|---|
| **Agent List** | See all agents, status (enabled/disabled), last triggered, error rate | P0 |
| **Agent Builder** | Create/edit agent YAML with syntax highlighting + validation | P0 |
| **AI Parser** | Text input for natural language, shows generated YAML | P0 |
| **Simulation** | Run mock event through agent, see decision path + actions | P0 |
| **Audit Viewer** | Search audit records by agent, merchant, time range, decision | P0 |
| **Agent Detail** | Metrics: evaluations/hour, decisions breakdown, action success rate | P1 |
| **Settings** | Cashfree API keys, notification channels, RBAC users | P1 |

### Not in v1
- Visual DAG editor (YAML with syntax highlighting is sufficient for v1)
- Real-time streaming dashboard
- Mobile responsive (desktop-first)

---

## 9. MVP Scope (12 Weeks)

### What Ships

| Week | Deliverable |
|---|---|
| 1-2 | Rule compiler: YAML to decision tree. Expression parser. |
| 3-4 | Agent runtime: event loop, rule evaluator, action dispatch. Go. |
| 5-6 | 3 Cashfree adapters: Disputes API, Payment Links API, Settlements API. |
| 7 | NATS integration. ClickHouse audit writer. |
| 8 | Docker Compose. Simulation mode. Health endpoints. |
| 9-10 | Dashboard: Agent list, builder, simulation, audit viewer. Next.js. |
| 11 | AI Parser integration (Claude API). |
| 12 | 3 production agents: Dispute Shield, Cart Recovery, Daily Ledger. |

### What Doesn't Ship in v1

| Feature | Why Not | When |
|---|---|---|
| Rust rule evaluator | Go is fast enough for <10K TPS. Add Rust when profiling demands it. | Phase 3 |
| Maker-checker UI | CLI approval is sufficient for v1. Dashboard approval in v2. | Phase 2 |
| Multi-cell deployment | Single instance handles 50K TPS. Cells needed at 100K+. | Phase 3 |
| On-prem bank deployment | Requires ISO 27001, security docs. | Phase 4 |
| 35 agents | 3 agents prove the pattern. Community builds the rest. | Ongoing |
| Cashfree Here adapter | Cashfree Here API is new. Stabilize first. | Phase 2 |

---

## 10. Success Metrics

### Phase 1 (MVP, Q2 2026)

| Metric | Target | How Measured |
|---|---|---|
| Time to first agent running | < 5 minutes from `git clone` | Manual test |
| GitHub stars (30 days) | > 1,000 | GitHub |
| Docker pulls (30 days) | > 5,000 | Docker Hub |
| Agents created (simulation) | > 500 | Telemetry (opt-in) |
| Community PRs | > 10 | GitHub |

### Phase 2 (Traction, Q3 2026)

| Metric | Target | How Measured |
|---|---|---|
| Merchants using (sandbox) | > 100 | Cashfree integration |
| Agents in production | > 50 (across all merchants) | Telemetry |
| Revenue impact tracked | > ₹10L recovered (disputes + carts) | Audit data |
| Community agents contributed | > 20 | GitHub |

### Phase 3 (Scale, Q4 2026)

| Metric | Target | How Measured |
|---|---|---|
| Merchants in production | > 1,000 | Cashfree AI Studio dashboard |
| Events processed/day | > 1M | Prometheus |
| p99 latency | < 10ms | Prometheus |
| Uptime | > 99.99% | StatusPage |

---

## 11. Open Questions (Needs Decision)

| Question | Options | Recommendation | Decision By |
|---|---|---|---|
| Should AI Parser use Claude or GPT? | Claude (better reasoning), GPT (cheaper, faster) | Claude for quality. Cost is $0.02/parse, negligible. | Engineering Lead |
| Should audit data include raw PII? | Yes (full audit), No (hash PII) | Hash by default. Full PII opt-in per agent with encryption. | Compliance |
| Should v1 support non-Cashfree APIs? | Yes (generic webhook adapter), No (Cashfree only) | Yes. Generic webhook adapter makes it truly open-source. Cashfree adapters are first-class. | Product Lead |
| Should the dashboard be part of the open-source repo? | Yes (MIT), No (commercial only) | Yes. Dashboard is the developer experience. Without it, adoption drops. | Leadership |
| Go module name? | github.com/cashfree/clawpay vs github.com/clawpay/clawpay | github.com/clawpay/clawpay (independent identity, Cashfree sponsors) | Leadership |

---

*This PRD defines what Clawpay v1 is, who it's for, how it works, and what success looks like. Everything else is intentionally deferred. Ship the first 3 agents. Get them running on real merchant data. Let the community tell us what to build next.*
