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

## Part 4: Additional Agents (Products Previously Missing)

These cover One Click Checkout, Cashfree Here (agentic payments), UPI Autopay, eNACH, Card EMI/Cardless EMI, BNPL, FlowWise Smart Routing, RiskShield (PG + Payouts), International Payments with Apple Pay, and Preauthorization.

| # | Agent Name | Cashfree Products | What It Does | Who Buys It |
|---|-----------|-------------------|-------------|-------------|
| 29 | **One-Click Conversion Agent** | One Click Checkout + Token Vault (Saved Cards) + UPI Autopay | Detects returning customers. Auto-surfaces saved payment instruments. Reduces checkout to single tap. Tracks conversion lift per instrument type. | D2C brands, e-commerce |
| 30 | **Cashfree Here Agent (Agentic Commerce)** | Cashfree Here (MCP) + Payment Gateway + Secure ID | The in-chat commerce agent. Customers browse, get recommendations, and pay inside the AI conversation. No redirect. No external checkout page. Verification inline if needed. | AI-native apps, chatbot commerce, Swiggy-style platforms |
| 31 | **UPI Autopay Recovery Agent** | UPI Autopay (Mandate Creation, Payment Scheduler) + Subscriptions API | Creates UPI mandates for recurring payments. On mandate failure: auto-retries at optimal time windows (payday, month-start). Sends WhatsApp nudge with one-tap re-authorize link. Tracks mandate creation-to-activation conversion. | SaaS, EdTech, insurance, OTT |
| 32 | **eNACH Lifecycle Agent** | eNACH (e-Mandate Creation, Activation, Debit) + Physical NACH | Creates e-mandates. Monitors activation (1-2 day window). On activation failure: falls back to Physical NACH or UPI Autopay. Schedules auto-debits. Retries failed debits with intelligent cadence. 98% success rate on subsequent EMIs. | NBFCs, lending, insurance |
| 33 | **EMI Optimizer Agent** | Card EMI (15+ issuers) + Cardless EMI (FlexMoney, ZestMoney, KreditBee) + BNPL (Simpl, LazyPay, ePayLater) + Offers API | On high-value cart: checks customer eligibility across all EMI/BNPL providers. Surfaces the best EMI option (lowest interest, longest tenure, no-cost). Dynamically shows BNPL options on product pages via BNPL Plus. Increases AOV. | E-commerce, D2C, electronics, furniture |
| 34 | **FlowWise Routing Agent** | FlowWise (Smart Routing, Threshold Routing, Volume-Based Routing, Dynamic Routing) | Monitors real-time success rates across all integrated PGs. Auto-routes transactions to the highest-performing gateway in the last 15/30 minutes. Falls back on threshold breach. Reports routing performance daily. Cuts processing cost up to 40%. | Any merchant with 2+ payment gateways |
| 35 | **RiskShield Gateway Agent** | RiskShield for PG (Shield Pro ML, Blacklists, Smart Limits, Velocity Checks, Device Fingerprinting) | Evaluates every transaction in real time. Blocks fraudulent accounts via ML. Enforces merchant-defined smart limits (frequency, cumulative sum per hour/day/week). Auto-blacklists bad actors. Reduces fraud by up to 40%. | All PG merchants |
| 36 | **RiskShield Payouts Agent** | RiskShield for Payouts (Disbursal Risk Scoring, Beneficiary Verification) | Screens every outbound payout against risk signals. Validates beneficiary via BAV before disbursement. Flags suspicious payout patterns. Blocks high-risk disbursals. Essential for lending, marketplace, and gig payouts. | NBFCs, marketplaces, gig platforms |
| 37 | **Preauth Hold Agent** | Preauthorization / Auth-Only Payments + Capture/Void | For hotel bookings, car rentals, subscription trials: places a hold on the card without charging. Monitors fulfillment events. Auto-captures when service delivered. Auto-voids if cancelled within window. Zero manual intervention. | Travel, hospitality, subscription trials |
| 38 | **International Checkout Agent** | International Payments (140+ currencies) + Apple Pay + Dynamic Currency Conversion | Detects international card BIN. Auto-shows price in customer's local currency. Enables Apple Pay for supported cards (reduces dropoff by up to 75%). Routes to optimal international acquiring bank. Reconciles FX settlements. | Export businesses, SaaS with global customers, ed-tech |
| 39 | **Net Banking & Wallet Router** | Net Banking (65+ banks) + Wallets (PhonePe, Paytm, Amazon Pay, Freecharge) | On checkout: detects customer's bank/wallet preference from past transactions. Pre-selects the most likely payment method. Falls back to next-best on failure. Tracks method-level success rates. | E-commerce, utilities, government payments |
| 40 | **Auto Collect Reconciler** | Auto Collect (Virtual Account creation, payment matching) | Creates unique virtual accounts per customer/invoice. Auto-matches incoming NEFT/IMPS/UPI to open invoices. Marks as paid. Triggers fulfillment. Sends receipt. Eliminates manual bank statement matching. | B2B, SaaS, educational institutions |
| 41 | **COD-to-Prepaid Converter** | Payment Links + RiskShield + UPI Intent | On COD order placement: runs RiskShield score. If high-risk: auto-generates a prepaid Payment Link with small incentive (discount/cashback). Sends via WhatsApp. If customer pays, converts to prepaid. If not, flags for ops review. Reduces RTO by 30-50%. | D2C, quick commerce, fashion e-commerce |

---

## Updated Full Count

| Category | Count | Razorpay Has? |
|----------|-------|--------------|
| Razorpay-matching agents | 8 | Yes (we match all 8) |
| Verify & Comply agents | 5 | No |
| Money Movement agents | 4 | No |
| Commerce & Ops agents | 2 | No |
| Deep Integration agents | 9 | No |
| **Checkout/Commerce/Routing/Risk agents (NEW)** | **13** | Partially (only RTO + cart) |
| **Total Clawpay agents** | **41** | **Razorpay has 8. We have 41.** |

---

## Cashfree Products Mapped (Complete List)

| Cashfree Product | Agent(s) Using It |
|-----------------|-------------------|
| **Payment Gateway** (Orders, Payments, OTP) | Cart Reclaimer, Conversational Checkout, RTO Guard, Offer Optimizer, FlowWise Routing, RiskShield Gateway |
| **One Click Checkout** | One-Click Conversion Agent |
| **Cashfree Here (Agentic Payments)** | Cashfree Here Agent |
| **Payment Links** | Cart Reclaimer, Bill Collection, Refund Dispatch, COD-to-Prepaid Converter |
| **Payment Forms** | Conversational Checkout |
| **UPI Autopay** | UPI Autopay Recovery Agent, Mandate Reviver |
| **eNACH / Physical NACH** | eNACH Lifecycle Agent |
| **Card EMI (15+ issuers)** | EMI Optimizer Agent |
| **Cardless EMI (FlexMoney, ZestMoney, KreditBee)** | EMI Optimizer Agent |
| **BNPL / Pay Later (Simpl, LazyPay, ePayLater)** | EMI Optimizer Agent |
| **Preauthorization / Auth-Only** | Preauth Hold Agent |
| **Token Vault / Saved Cards** | Card Vault Intelligence, One-Click Conversion Agent |
| **Net Banking (65+ banks)** | Net Banking & Wallet Router |
| **Wallets (PhonePe, Paytm, Amazon Pay)** | Net Banking & Wallet Router |
| **Subscriptions / Mandates** | Mandate Reviver, UPI Autopay Recovery, eNACH Lifecycle |
| **Offers & Eligibility** | Offer Optimizer, EMI Optimizer |
| **FlowWise (Smart Routing / Orchestration)** | FlowWise Routing Agent |
| **RiskShield for PG** | RiskShield Gateway Agent, RTO Guard, COD-to-Prepaid Converter |
| **RiskShield for Payouts** | RiskShield Payouts Agent |
| **Disputes API** | Dispute Shield |
| **Refunds API** | RTO Lens, Refund Dispatch |
| **Instant Refunds** | Refund Dispatch |
| **Settlements / Reconciliation** | CashRadar, Daily Ledger, RTO Lens, CrossBorder Settlement |
| **Payouts (Direct/Batch/Internal/CardPay)** | Allocation Agent, Payroll Autopilot, Marketplace Disbursement, RiskShield Payouts |
| **Easy Split** | Marketplace Disbursement, Daily Ledger |
| **OneEscrow** | Escrow Orchestrator |
| **Auto Collect** | Auto Collect Reconciler, Inward Reconciler |
| **Cashgram** | Mandate Reviver, Refund Dispatch |
| **Virtual Banking (VBA)** | Inward Reconciler, Auto Collect Reconciler |
| **Secure ID (Full Suite)** | Compliance Agent, Re-KYC Enforcer, Instant KYC, Credit Pre-Screen, Mule Detector, Dispute Shield, RTO Guard, Cashfree Here Agent |
| **1-Click Onboarding (Mobile360)** | Instant KYC |
| **Account Aggregator** | Credit Pre-Screen |
| **E-Sign** | E-Sign Closer, Re-KYC Enforcer |
| **DigiLocker** | Compliance Agent |
| **BBPS** | Bill Collection Agent |
| **PPI Wallet** | Wallet Lifecycle Agent |
| **SoftPOS** | Field Ops Monitor |
| **Platforms (Sub-Merchant Onboarding)** | Sub-Merchant Activator |
| **International Payments (140+ currencies)** | CrossBorder Settlement Agent, International Checkout Agent |
| **Apple Pay (International Cards)** | International Checkout Agent |

**41 agents. 39 Cashfree products. Every single product has at least one Clawpay agent. No product left behind.**
