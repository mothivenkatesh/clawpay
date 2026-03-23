# Clawpay Agent Catalog

**Theme: Agentic Commerce & Payments Innovation**
**Date:** March 24, 2026

Razorpay launched 8 AI agents at FTX'26. All post-transaction.
Clawpay maps 19+ agents across the FULL Cashfree product surface. Pre-transaction, during-transaction, and post-transaction.

---

## The Framing

Razorpay's pitch: "Without us, AI just talks; with us, it transacts."
Clawpay's pitch: **"Razorpay agents recover money. Clawpay agents move it."**

Razorpay Agent Studio = 8 agents, all post-payment intelligence.
Clawpay = 19+ agents across verify, collect, disburse, split, monitor, comply.

---

## Part 1: Matching Razorpay's 8 Agents (Same Capabilities, Cashfree Rails)

| # | Razorpay Agent | Clawpay Equivalent | Cashfree APIs Used | What's Better |
|---|---------------|-------------------|-------------------|--------------|
| 1 | **Dispute Responder** | **Dispute Shield** | Disputes API (Accept, Get, Submit Evidence) + Dispute Webhooks | Integrates Secure ID face-match as evidence for disputed KYC transactions. Razorpay can't do this. |
| 2 | **Subscription Recovery** | **Mandate Reviver** | Subscriptions API (Raise Payment, Retry, Manage) + Cashgram | Can generate Cashgram payout link for partial refund/credit to re-engage churned subscribers. |
| 3 | **Abandoned Cart (SuperU)** | **Cart Reclaimer** | Payment Links API (Create, Get, Cancel) + Webhooks | Payment Links have richer customization. Time-limited discount baked into the link itself. |
| 4 | **Abandoned Cart (Nugget)** | **Conversational Checkout** | Payment Forms API + Payment Links | Embeds Payment Form in WhatsApp chat. Cart completion without leaving the conversation. |
| 5 | **Cashflow Forecaster** | **CashRadar** | Settlements API (Get, Get for Order) + Payouts Balance API (Ledger) + Reconciliation API | Includes payout commitments in the forecast. Razorpay only looks at inflows. |
| 6 | **RTO Shield** | **RTO Guard** | Risk Management API (Submit Risk Details) + Secure ID Device Intelligence | Layers Secure ID device/IP signals on top of address validation. Deeper fraud intelligence. |
| 7 | **RTO Insights** | **RTO Lens** | Settlements Reconciliation + Refunds API (Get for Order) | Weekly scheduled report. Surfaces top-10 return drivers by pincode, SKU, customer. |
| 8 | **Settlement Insights** | **Daily Ledger** | Settlements API + Easy Split Vendor Reconciliation | Includes vendor split settlements in the same message. Razorpay can't do marketplace reconciliation. |

**Score: 8/8 matched. 6/8 with Cashfree-specific advantages Razorpay can't replicate.**

---

## Part 2: 11 Agents Razorpay Doesn't Have (Cashfree-Exclusive Surface)

### Verify & Comply Agents

| # | Agent Name | Cashfree Products | What It Does | Who Buys It |
|---|-----------|-------------------|-------------|-------------|
| 9 | **Compliance Agent** | Secure ID: PAN 360, Aadhaar OCR, FaceMatch, Liveness, CKYC, DigiLocker, Video KYC | Pre-configured KYC journeys. CDD, EDD, V-CIP, mule screening. Full verify > decide flow. 6 vendors become one config. | NBFCs, Banks, Fintechs |
| 10 | **Re-KYC Enforcer** | Secure ID: All verification APIs + E-Sign | Periodic agent. Identifies customers with expiring KYC. Auto-triggers re-verification. Suspends access if not renewed. Addresses RBI's penalty risk for outdated profiles. | Banks, NBFCs (62 penalized in 2023-2025) |
| 11 | **Instant KYC (1-Click)** | Secure ID: 1-Click Onboarding SDK (Mobile360), Bureau, EPFO, CIBIL | Single mobile number triggers full identity + credit pre-check. Onboarding from 3 days to 30 seconds. | Digital lenders, wallets, BNPL |
| 12 | **Credit Pre-Screen** | Account Aggregator APIs (Consent, Financial Info) + Secure ID | On loan application: requests AA consent, pulls bank statements, runs income rules, auto-approves or flags. Connects to Payouts for disbursement. | NBFCs, lending startups |
| 13 | **Mule Detector** | Secure ID: Device Intelligence, IP Risk, BAV, FaceMatch + RiskShield | Real-time mule scoring at onboarding + T+1/T+7/T+30 behavioral monitoring. Network graph analysis. Geographic mismatch detection. | Banks (RBI mandate), lenders |

### Money Movement Agents

| # | Agent Name | Cashfree Products | What It Does | Who Buys It |
|---|-----------|-------------------|-------------|-------------|
| 14 | **Allocation Agent** | Payouts API (Direct/Batch Transfer) + Easy Split + OneEscrow | Co-lending splits, escrow, TDS, conditional disbursement, time-based release. Everything finance teams do in Excel. | NBFCs, co-lending platforms |
| 15 | **Marketplace Disbursement** | Easy Split (Vendor Create, Split After Payment, On-Demand Transfer) | Watches completed orders. Calculates platform fee + vendor share. Auto-disburses on custom schedules. Delivers vendor settlement report. | B2B marketplaces, aggregators |
| 16 | **Escrow Orchestrator** | OneEscrow (Create VA, Allocate Funds, Transfer) | Per-transaction escrow. Holds funds until conditions met (delivery confirmed, milestone hit, KYC passed). Auto-releases via Payouts. | Marketplaces, real estate, freelance platforms |
| 17 | **Refund Dispatch** | Cashgram (Create, Get Status) + Refunds API | When you don't know the customer's bank account: auto-generates Cashgram link, sends via SMS/WhatsApp, verifies beneficiary name before release. Handles bulk refund campaigns. | E-commerce, insurance, product recalls |

### Commerce & Operations Agents

| # | Agent Name | Cashfree Products | What It Does | Who Buys It |
|---|-----------|-------------------|-------------|-------------|
| 18 | **Inward Reconciler** | Virtual Banking (Create VBA, Get Payments by UTR) + Settlements | Creates unique VBAs per customer/invoice. Watches for inbound payments by UTR. Auto-matches to open invoice. Marks paid. Triggers fulfillment. Eliminates manual bank statement reconciliation. | B2B businesses, SaaS |
| 19 | **Bill Collection Agent** | BBPS APIs + Payment Links | Monitors due dates per customer. Sends pre-due reminders via WhatsApp. Generates Payment Link. Confirms payment via BBPS status. Updates biller system. | Utilities, insurance, subscription businesses |

---

## Part 3: Bonus Agents (Deeper Cashfree Product Integration)

| # | Agent Name | Cashfree Products | What It Does |
|---|-----------|-------------------|-------------|
| 20 | **Sub-Merchant Activator** | Platforms API (Create Merchant, Onboarding Links, KYC Status, Doc Upload) | Watches new sub-merchant registrations. Triggers KYC docs. Polls activation status. Escalates stalled applications. |
| 21 | **Field Ops Monitor** | SoftPOS (Create Terminal, Transactions, QR Codes) | Monitors terminal transaction volumes. Flags dormant terminals. Daily settlement per terminal. Alerts on suspicious patterns. |
| 22 | **Wallet Lifecycle Agent** | PPI Wallet (Create, Credit, Debit, KYC, Close) | Automates min-KYC to full-KYC upgrade. Monitors balance thresholds. Triggers top-up reminders. Handles closure with fund repatriation. |
| 23 | **Payroll Autopilot** | Payouts Batch Transfer + BAV | On payroll date: pulls employee list, validates bank accounts, executes batch transfer, reports failures. Handles TDS deductions pre-disbursement. |
| 24 | **CrossBorder Settlement Agent** | International Payments (ICA Settlement, Reconciliation, Payment Verification) | Monitors inbound international settlements. Verifies payment details. Reconciles against orders. Alerts on FX rate movements. |
| 25 | **Card Vault Intelligence** | Cards & Token Vault (Tokenization, BIN Details, PAR) | Monitors tokenized card usage patterns. Flags expired tokens before subscription renewal. Pre-fetches cryptograms for high-value recurring. |
| 26 | **Offer Optimizer** | Offers & Eligibility API (Create Offers, Get Eligible Methods/EMI/PayLater) | Dynamically serves the best payment offer based on cart value, customer segment, and payment method eligibility. Maximizes conversion. |
| 27 | **RiskShield Agent** | Risk Management API + Secure ID | Real-time fraud scoring on every transaction. Auto-blocks high-risk payments. Monitors downtime incidents. Auto-routes traffic during outages. |
| 28 | **E-Sign Closer** | E-Sign API (Upload, Create Request, Get Status) | After KYC passes and loan approved: auto-triggers agreement e-sign. Monitors completion. Blocks disbursement until signed. Full audit trail. |

---

## The Full Count

| Category | Count | Razorpay Agent Studio Has? |
|----------|-------|--------------------------|
| **Razorpay-matching agents** | 8 | Yes (we match all 8) |
| **Verify & Comply agents** | 5 | No |
| **Money Movement agents** | 4 | No |
| **Commerce & Ops agents** | 2 | No |
| **Bonus/Deep Integration agents** | 9 | No |
| **Total Clawpay agents** | **28** | Razorpay has 8. We have 28. |

---

## The Narrative

Razorpay Agent Studio: 8 agents. All post-transaction. Disputes, cart recovery, RTO, forecasting. Smart stuff. But limited to "what happens after money moves."

Clawpay: 28 agents across the full Cashfree stack. Verify, comply, collect, split, disburse, reconcile, monitor. Pre-transaction, during-transaction, AND post-transaction.

**Razorpay agents recover revenue. Clawpay agents move it.**

The broader theme: **Agentic Commerce & Payments Innovation.** The idea that every financial workflow, from KYC to disbursement to reconciliation, should have an AI agent that observes, decides, and acts. Not a dashboard you check. Not an API you call. An agent that runs autonomously on your behalf.

Razorpay started this conversation. Clawpay finishes it.

---

## Cashfree Products Mapped (Complete List)

| Cashfree Product | Agent(s) Using It |
|-----------------|-------------------|
| Payment Gateway (Orders, Payments) | Cart Reclaimer, Conversational Checkout, RTO Guard, Offer Optimizer |
| Payment Links | Cart Reclaimer, Bill Collection, Refund Dispatch |
| Payment Forms | Conversational Checkout |
| Subscriptions / Mandates | Mandate Reviver |
| Cards & Token Vault | Card Vault Intelligence |
| UPI / Virtual Banking | Inward Reconciler |
| Disputes API | Dispute Shield |
| Refunds API | RTO Lens, Refund Dispatch |
| Settlements / Reconciliation | CashRadar, Daily Ledger, RTO Lens, CrossBorder Settlement |
| Risk Management | RTO Guard, RiskShield Agent |
| Offers & Eligibility | Offer Optimizer |
| Payouts (Direct/Batch/Internal) | Allocation Agent, Payroll Autopilot, Marketplace Disbursement |
| Easy Split | Marketplace Disbursement, Daily Ledger |
| OneEscrow | Escrow Orchestrator |
| Cashgram | Mandate Reviver, Refund Dispatch |
| Secure ID (Full Suite) | Compliance Agent, Re-KYC Enforcer, Instant KYC, Credit Pre-Screen, Mule Detector, Dispute Shield, RTO Guard |
| 1-Click Onboarding (Mobile360) | Instant KYC |
| Account Aggregator | Credit Pre-Screen |
| E-Sign | E-Sign Closer, Re-KYC Enforcer |
| BBPS | Bill Collection Agent |
| PPI Wallet | Wallet Lifecycle Agent |
| SoftPOS | Field Ops Monitor |
| Platforms (Sub-Merchant) | Sub-Merchant Activator |
| International Payments | CrossBorder Settlement Agent |
| Flowwise | All agents (simplified integration layer) |

**Every Cashfree product has at least one Clawpay agent. No product left behind.**
