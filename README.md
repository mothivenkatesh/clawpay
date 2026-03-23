# Clawpay + Cashfree AI Studio

**Clawpay** = open-source agentic payments engine (MIT).
**Cashfree AI Studio** = commercial product built on it.

35 AI agents across every Cashfree product. Full lifecycle. Open-core engine + commercial platform.

**Theme: Agentic Commerce & Payments Innovation**

## [View the Deck](https://mothivenkatesh.github.io/clawpay/)

---

## Why Two Layers

| Company | Open Layer | Commercial Layer | Result |
|---------|-----------|-----------------|--------|
| HashiCorp | Terraform | Terraform Cloud | $14B IPO |
| Elastic | Elasticsearch | Elastic Cloud | $4.6B IPO |
| MongoDB | MongoDB | Atlas | $30B+ |
| Juspay | Hyperswitch | HyperPG | 40K stars |
| **Cashfree** | **Clawpay** | **Cashfree AI Studio** | |

Razorpay Agent Studio: 8 agents. Proprietary. No community.
Cashfree: 35 agents. Open-core. Ecosystem.

**Razorpay has a product. Cashfree would have a movement.**

---

## 35 Agents by Category

### Payment & Checkout (9 agents)

| Agent | Pain It Solves | Cashfree APIs |
|-------|---------------|---------------|
| **Payment Failure Diagnostics** | Merchant sees "failed." Doesn't know why. Customer gone. Agent: instant root cause + WhatsApp alert with fix. | PG, Incidents, Downtimes |
| **Smart Retry** | UPI timeout. Customer thinks it failed. Leaves. Agent: waits 90s, generates retry link with next-best method. Avoids double charge. | Order Pay, Payment Links, Subscriptions |
| **FlowWise Routing** | Wrong gateway = lower success rate + higher MDR. Agent: auto-routes to best gateway in real-time per 15-min window. | FlowWise (Dynamic, Smart, Threshold, Volume Routing) |
| **RiskShield Fraud** | Fraud slips through (chargeback) OR good customer blocked (false positive). Agent: ML scoring + rules. Auto-adjusts thresholds on false positive patterns. | RiskShield, Secure ID Device Intelligence |
| **Dispute Shield** | Dispute arrives. Ops sees it on day 6 of 7. Scrambles. Loses. Agent: auto-submits evidence within 24 hours. Win rate: 20% to 50%. | Disputes API, Webhooks |
| **One-Click Conversion** | Returning customer re-enters card details. Drops off. Agent: detects returning customer, surfaces saved instruments. One tap. +23% conversion. | One Click Checkout, Token Vault |
| **Cart Recovery** | 70-80% cart abandonment. Most merchants don't follow up. Agent: WhatsApp Payment Link 30 min after abandonment. Recovers 5-10%. | Payment Links |
| **Cashfree Here (Agentic Commerce)** | AI chatbot recommends product. Customer has to leave chat to pay. 60% drop off. Agent: payment inside the conversation. No redirect. | Cashfree Here (MCP), PG, Secure ID |
| **EMI/BNPL Optimizer** | Customer sees ₹45K laptop. Can't pay full amount. Leaves. Agent: checks eligibility across 15+ EMI issuers + BNPL. Shows best option on product page. AOV +34%. | Offers, Card EMI, Cardless EMI, BNPL |

### Money Movement (7 agents)

| Agent | Pain It Solves | Cashfree APIs |
|-------|---------------|---------------|
| **Allocation Agent** | Marketplace does multi-party splits in Excel. Manual payout. Reconciliation nightmare. Agent: auto-calculates splits, TDS, commission. Auto-disburses per vendor schedule. | Payouts, Easy Split, OneEscrow |
| **Beneficiary Verification** | 3% of payouts fail (wrong bank details). Some are fraud. Agent: BAV + name match before EVERY payout. Catches errors + fraud. | Payouts, BAV, IFSC |
| **Payroll Autopilot** | Monthly chaos. Finance validates accounts manually. 5-10 failures. Agent: bulk BAV, batch transfer, auto-retry failures, TDS report. | Payouts Batch, BAV |
| **Instant Refund** | Refund takes 5-7 days. Customer calls 3 times. 1-star review. Agent: instant refund for eligible. Cashgram link for COD. | Refunds, Cashgram |
| **Marketplace Split** | 3 vendors on one order. Different commission rates. Different settlement schedules. Agent: auto-calculates, auto-disburses, vendor WhatsApp report. | Easy Split (all APIs) |
| **Escrow Orchestrator** | Freelance platform holds money in main account. If platform goes bankrupt, money's mixed. Agent: OneEscrow per transaction. Hold until milestone. Auto-release. | OneEscrow, Payouts |
| **RTO Guard + Insights** | 25% COD orders return. ₹150-300 per RTO. Agent: risk scores COD orders. High-risk auto-converts to prepaid via Payment Link. Weekly pattern report. | Risk Management, Secure ID, Payment Links, Refunds |

### Verify & Comply (8 agents)

| Agent | Pain It Solves | Cashfree APIs |
|-------|---------------|---------------|
| **KYC Onboarding** | 6 separate API integrations for one onboarding. 40% customer drop-off during KYC. Agent: PAN + Aadhaar + FaceMatch + Liveness + BAV in one config. 30 seconds. | Secure ID (full suite) |
| **Mule Detection** | 90% of mule accounts pass standard KYC (identity is real, intent is fraud). Agent: device velocity, geo mismatch, SIM age + T+1/T+7/T+30 behavioral monitoring. | Secure ID, RiskShield, Payouts Intelligence |
| **Re-KYC Enforcer** | 20% of bank customers have KYC older than 2 years. Nobody tracks. RBI audit finds gaps. Fine. Agent: auto-triggers re-verification 60 days before expiry. WhatsApp nudges. Auto-restricts on lapse. | Secure ID, E-Sign |
| **AML Case Manager** | 500 AML alerts/day. 85% false positives. 45 min per case. Agent: auto-enriches, auto-classifies, auto-closes false positives with audit trail. 75% reduction in analyst time. | Secure ID (AML/PEP/Sanctions, Adverse Media, Network Analysis) |
| **Sub-Merchant KYC** | Platform onboards 100 merchants/month. 7-10 day activation. Agent: auto-validates docs. 56% auto-activated. Activation: 8 days to 2 hours. | Platforms (all APIs) |
| **Continuous Risk Monitoring** | Customer was clean at onboarding. 6 months later: on sanctions list. Bank doesn't know until annual review. Agent: daily batch against updated databases. Instant alert on new match. | Secure ID, RiskShield |
| **Dynamic Offer Optimizer** | Blanket "10% off" for everyone. Some customers would pay full price. Agent: per-customer offer selection based on segment, cart value, history. Same conversion, lower cost. | Offers, Payment Links, Subscriptions |
| **Wallet Lifecycle** | 60-70% of min-KYC wallets never upgrade. Capped at ₹10K. Agent: upgrade nudges at 80% limit. Dormancy reactivation. Closure + fund repatriation. | PPI Wallet (all APIs) |

### Recurring, Recon & Ops (11 agents)

| Agent | Pain It Solves | Cashfree APIs |
|-------|---------------|---------------|
| **UPI Autopay Lifecycle** | 30% mandates never activated. 15% debits fail silently. Silent churn. Agent: activation nudges, optimal retry windows, WhatsApp re-authorize link. | Subscriptions, UPI Autopay |
| **eNACH/NACH Agent** | 20% mandate activation failures. Wrong bank details. Agent: BAV first, then eNACH. On failure: auto-falls back to UPI Autopay, then Physical NACH. | Subscriptions, eNACH, BAV |
| **Mandate Reviver** | 500 subscription payments fail/month. Most customers don't know. Silent churn. Agent: retries at optimal time, WhatsApp nudge, Cashgram fallback. 40-50% recovery. | Subscriptions, Cashgram |
| **Inward Reconciliation** | 200 payments/day via bank transfer. Finance manually matches to invoices. 2-3 hours daily. Agent: VBA per customer, auto-match by UTR, triggers fulfillment. | Virtual Banking (VBA) |
| **CashRadar (Forecast)** | Friday afternoon. Payroll Monday. "Do we have enough cash?" Nobody knows. Agent: 7-day rolling forecast. Inflows + outflows + settlements. Alerts before shortfall. | Settlements, Payouts Balance, Easy Split |
| **Daily Ledger** | Merchant logs into dashboard every morning. 30 minutes checking numbers. Agent: morning WhatsApp: gross, fees, net settled, pending. Done. | Settlements, Reconciliation |
| **CrossBorder Settlement** | FX rate moves 2% before settlement. Merchant didn't know. Agent: tracks rate, alerts on movement, auto-uploads verification, diagnoses delays. | International Payments, Global Collections |
| **International Checkout** | International card declines 2-3x higher than domestic. Agent: detects intl BIN, shows local currency (DCC), enables Apple Pay. Reduces dropoff 75%. | International (140+ currencies), Apple Pay, BIN |
| **Field Ops Monitor** | 50 stores with SoftPOS. Head office has no visibility. Which terminals are active? Agent: daily health scan, flags dormant devices, per-store settlement report. | SoftPOS (all APIs) |
| **Bill Collection** | 30% of premiums paid late. Manual reminder calls cost ₹15-20 each. Agent: WhatsApp reminders + Payment Link 7 days before due. On-time collection: 70% to 89%. | BBPS, Payment Links |
| **Auto Collect Reconciler** | B2B payments via NEFT. Manual bank statement matching. Unmatched payments sit for weeks. Agent: VBA per invoice, auto-match by UTR, triggers receipt. | Auto Collect, Virtual Banking |

---

## Quick Start

```bash
docker pull clawpay/gateway
docker-compose up -d
```

### AI Parser

```
"Verify vendor PAN + GSTIN, check bank account,
 split payment 85% vendor / 15% platform,
 deduct TDS, payout weekly"
```

Generates:

```yaml
trigger: webhook/order-complete
steps:
  - verify: pan_360
  - verify: gstin
  - verify: bank_account
  - split:
      vendor: 85%
      platform: 15%
      tds: auto
  - execute: cashfree/easy-split
```

---

## Architecture

```
CLAWPAY (Open)                          CASHFREE AI STUDIO (Commercial)
MIT licensed engine                     Managed, Cashfree-native
Any developer, any rail                 8L+ merchants, one-click enable
Community agents                        Enterprise support + SLA
GitHub stars = mindshare                Revenue = txn fees + agent fees
Sets the STANDARD                       Monetizes the STANDARD
```

99% deterministic rules (zero token cost). 1% AI edge cases. AI parser for config (one-time).

---

## Competition

| | Clawpay + AI Studio | Razorpay Agent Studio | HyperVerge | Lentra | Perfios |
|---|---|---|---|---|---|
| Agents | **35** | 8 | 0 | 0 | 0 |
| Scope | Full lifecycle | Post-txn only | Verify only | Decide only | Data only |
| Open Source | MIT core | Proprietary | No | No | No |
| On-Prem | Docker, hours | Cloud-only | ? | Cloud-only | Yes |
| Runtime Cost | Zero tokens | Tokens/action | Per API | License | License |

---

## Market

- **$19.3B** Decision Management TAM by 2032
- **$133B** India digital lending revenue by 2030
- **62 NBFCs** penalized by RBI (2023 to 2025)
- **87%** India fintech adoption (vs 67% global)
- **8L+** existing Cashfree merchants to activate

---

## Roadmap

| Track | Q2 2026 | Q3 2026 | Q4 2026+ |
|-------|---------|---------|----------|
| **Clawpay (Open)** | Core engine on GitHub (MIT) | Community agent templates | Agentic payments standard |
| **AI Studio (Commercial)** | First 8 agents for 8L+ merchants | Expand to 20+ agents | 35 agents + enterprise on-prem |

---

## Files

| File | What |
|------|------|
| `index.html` | 16-slide deck |
| `clawpay-agent-catalog-2026-03-24.md` | Full agent catalog with pain descriptions |
| `clawpay-product-vision-2026-03-23.md` | Research-backed vision document |

---

## License

Clawpay core: MIT. Cashfree AI Studio: Proprietary.

---

*Razorpay has a product. Cashfree would have a movement.*
