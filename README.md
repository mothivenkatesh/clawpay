# Clawpay + Cashfree AI Studio

**Clawpay** = open-source agentic payments engine (MIT).
**Cashfree AI Studio** = commercial product built on it.

Clawpay sets the standard. Cashfree AI Studio monetizes it.

41 AI agents across 39 Cashfree products. Pre-transaction, during-transaction, and post-transaction.

**Theme: Agentic Commerce & Payments Innovation**

## [View the Deck](https://mothivenkatesh.github.io/clawpay/)

---

## Why Two Layers

Every company that became the category leader in developer infrastructure did the same thing:

| Company | Open Layer | Commercial Layer | Result |
|---------|-----------|-----------------|--------|
| HashiCorp | Terraform | Terraform Cloud | $14B IPO |
| Elastic | Elasticsearch | Elastic Cloud | $4.6B IPO |
| MongoDB | MongoDB | Atlas | $30B+ market cap |
| Juspay | Hyperswitch | HyperPG | 40K GitHub stars |
| **Cashfree** | **Clawpay** | **Cashfree AI Studio** | ? |

Razorpay Agent Studio is proprietary. 8 agents. Closed. No community. No ecosystem.

**Cashfree open-sources the engine. That's the category leadership move.**

Razorpay has a product. Cashfree would have a movement.

---

## The Problem

Razorpay built 8 AI agents for **after** money moves. Disputes, cart recovery, RTO, cashflow.

Nobody built agents for **before** money moves. Verify identity. Check eligibility. Calculate splits. Disburse. Comply with RBI.

Right now: 4-6 vendors, Excel sheets, 3+ days of someone pushing buttons.

---

## 41 Agents Across Every Cashfree Product

### Matching Razorpay (8 agents)
Dispute Shield, Cart Reclaimer, Mandate Reviver, CashRadar, RTO Guard, RTO Lens, Daily Ledger, Conversational Checkout.

All 8 of Razorpay's agents replicated on Cashfree rails. 6 of 8 with Cashfree-specific advantages Razorpay can't replicate.

### Verify & Comply (5 agents, Razorpay has zero)
Compliance Agent, Re-KYC Enforcer, Instant KYC, Credit Pre-Screen, Mule Detector.

### Move Money (4 agents, Razorpay has zero)
Allocation Agent, Marketplace Disbursement, Escrow Orchestrator, Refund Dispatch.

### Commerce, Checkout & Risk (24 agents)
One-Click Conversion, Cashfree Here (agentic commerce), UPI Autopay Recovery, eNACH Lifecycle, EMI Optimizer, FlowWise Routing, RiskShield (PG + Payouts), Preauth Hold, International Checkout, Net Banking & Wallet Router, Auto Collect Reconciler, COD-to-Prepaid Converter, Bill Collection, Sub-Merchant Activator, Field Ops Monitor, Wallet Lifecycle, Payroll Autopilot, CrossBorder Settlement, Card Vault Intelligence, Offer Optimizer, E-Sign Closer, Inward Reconciler, RTO Insights, Settlement Insights.

---

## Quick Start (Clawpay Open Layer)

```bash
docker pull clawpay/gateway
docker-compose up -d
# Open browser > Build flow > Simulate > Deploy
```

### AI Parser

```
"Verify Aadhaar, match face, check CIBIL > 680,
 disburse 80/20 co-lending split"
```

Generates:

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

## Architecture

```
OPEN LAYER (Clawpay)                    COMMERCIAL LAYER (Cashfree AI Studio)
- MIT licensed engine on GitHub          - Managed, hosted, Cashfree-native
- Any developer can use it               - 8L+ merchants activate with one click
- Works with any payment rail            - Deepest Cashfree integration
- Community contributes agents           - Enterprise support + SLA
- GitHub stars = developer mindshare     - Revenue = txn fees + agent fees
- Sets the STANDARD                      - Monetizes the STANDARD
```

### Why Deterministic Rules, Not LLM-Per-Action

Razorpay burns tokens on every agent action (Claude SDK). Works for 50 cart recoveries/day.

For 10M accounts in continuous monitoring, LLM costs would be astronomical.

Clawpay: 99% deterministic rules (zero cost). 1% AI edge cases (worth the tokens). AI parser for config generation (one-time).

---

## Cashfree Products Covered (All 39)

Payment Gateway, One Click Checkout, Cashfree Here, Payment Links, Payment Forms, UPI Autopay, eNACH/Physical NACH, Card EMI (15+ issuers), Cardless EMI, BNPL/Pay Later, Preauthorization, Token Vault/Saved Cards, Net Banking (65+ banks), Wallets (PhonePe/Paytm/Amazon Pay), Subscriptions/Mandates, Offers & Eligibility, FlowWise (Smart Routing), RiskShield for PG, RiskShield for Payouts, Disputes, Refunds, Instant Refunds, Settlements/Reconciliation, Payouts (Direct/Batch/Internal/CardPay), Easy Split, OneEscrow, Auto Collect, Cashgram, Virtual Banking, Secure ID (full suite), 1-Click Onboarding, Account Aggregator, E-Sign, DigiLocker, BBPS, PPI Wallet, SoftPOS, Platforms (Sub-Merchant), International Payments + Apple Pay.

**Every single Cashfree product has at least one agent. No product left behind.**

---

## Competition

| | Clawpay + AI Studio | Razorpay Agent Studio | HyperVerge | Lentra | Perfios |
|---|---|---|---|---|---|
| **Agents** | 41 | 8 | 0 | 0 | 0 |
| **Scope** | Full lifecycle | Post-transaction only | Verify only | Decide only | Data only |
| **Open Source** | MIT core | Proprietary | No | No | No |
| **On-Prem** | Docker, hours | Cloud-only | Unconfirmed | Cloud-only | Available |
| **Runtime Cost** | Zero tokens | Tokens per action | Per API | License | License |
| **Community** | GitHub + contributors | None | None | None | None |

---

## Market

- **$19.3B** Decision Management TAM by 2032
- **$133B** India digital lending revenue by 2030
- **62 NBFCs** penalized by RBI (2023-2025)
- **87%** India fintech adoption (vs 67% global)
- **41** agents mapped, **39** products covered, **8L+** existing merchants to activate

---

## Roadmap

**Clawpay (Open):**
- Q2 2026: Core engine on GitHub (MIT). Rule engine + AI parser + simulation.
- Q3 2026: Community agent templates. Conference talks. Developer advocacy.
- Q4 2026+: Becomes the agentic payments standard.

**Cashfree AI Studio (Commercial):**
- Q2 2026: First 8 agents (match Razorpay). One-click enable for 8L+ merchants.
- Q3 2026: Expand to 20+ agents. Verify + money movement + commerce.
- Q4 2026+: 41 agents. Enterprise on-prem. Top 25 banks.

---

## Files

| File | What |
|------|------|
| `index.html` | 15-slide deck. Arrow keys or swipe. |
| `clawpay-agent-catalog-2026-03-24.md` | Full 41-agent catalog with Cashfree product mapping. |
| `clawpay-product-vision-2026-03-23.md` | Research-backed vision document. |

---

## License

Clawpay core engine: MIT License
Cashfree AI Studio: Proprietary (Cashfree Payments)

---

*Razorpay has a product. Cashfree would have a movement.*
