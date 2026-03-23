# Clawpay Agent Catalog

**Agentic Commerce & Payments Innovation**
**Date:** March 24, 2026

Clawpay (open-source engine) + Cashfree AI Studio (commercial product).
Every agent solves a real pain that costs merchants money today.

---

## The Rule: No Agent Without a Real Pain

Every agent listed here passes one test: **"Is someone losing money, time, or customers because this doesn't exist today?"**

If the answer is "it would be nice," it's not here. If the answer is "we lost ₹3L last month because of this," it's here.

---

## PAYMENT GATEWAY AGENTS

### 1. Payment Failure Diagnostics Agent
**Cashfree APIs:** Get Payment by ID, Get Payments for Order, Incident Service, Downtime APIs
**The pain:** Merchant sees "payment failed" in dashboard. Doesn't know why. Was it the bank? The card network? UPI timeout? Customer's phone? They raise a support ticket. Wait 24 hours. Meanwhile, the customer bought from a competitor.
**What the agent does:** On every failed payment webhook, instantly diagnoses root cause (bank decline code, network timeout, OTP expiry, insufficient funds, etc.). Sends merchant a WhatsApp/Slack alert: "Payment ₹4,500 failed. Reason: HDFC Bank declined (insufficient funds). Customer's card has failed 3 times this month. Suggest: send Payment Link with UPI option." Tracks failure patterns by bank, payment method, time of day. Weekly report: "Your ICICI card success rate dropped 12% this week. Here's why."
**Who loses money without this:** Every merchant. Payment failures = lost revenue. Most merchants never investigate WHY.

### 2. Smart Retry Agent
**Cashfree APIs:** Order Pay, Payment Links (Create), Subscriptions (Raise Payment, Retry)
**The pain:** Customer's UPI payment times out. They think the payment failed. They leave. Merchant lost the sale. Or worse: customer tries again, gets charged twice, files a dispute.
**What the agent does:** On UPI timeout/bank decline, waits 90 seconds (avoids double charge), then auto-generates a new Payment Link with the next-best payment method pre-selected (if UPI failed, show card/net banking). Sends via SMS. If the customer already paid (late callback), detects the duplicate and auto-cancels the retry link. Tracks retry conversion rate.
**Who loses money without this:** D2C brands, subscription businesses. Industry data: 15-20% of "failed" UPI transactions actually succeed with late callbacks. Without smart retry, merchants lose these customers.

### 3. FlowWise Routing Agent
**Cashfree APIs:** FlowWise (Dynamic Routing, Smart Routing, Threshold Routing, Volume-Based Routing)
**The pain:** Merchant uses 2-3 payment gateways. Manually routes traffic. Doesn't know which gateway has better success rate RIGHT NOW. Pays higher MDR on one gateway when the other would have been cheaper AND faster.
**What the agent does:** Monitors real-time success rates per gateway per payment method per bank (last 15 min window). Auto-routes every transaction to the highest-performing gateway. On gateway downtime, instant failover. Weekly report: "Switched 34% of HDFC card traffic from Gateway A to Gateway B. Success rate improved 8%. MDR saved ₹1.2L this week."
**Who loses money without this:** Any merchant processing 1,000+ transactions/day with multiple gateways. FlowWise already does this, but the AGENT layer adds the intelligence of when to override rules, when to alert, and when to renegotiate MDR with the underperforming gateway.

### 4. RiskShield Fraud Agent
**Cashfree APIs:** RiskShield (Submit Risk Details, Smart Rules, Blacklists, Smart Limits), Secure ID (Device Intelligence)
**The pain:** Two failure modes. (A) Fraud slips through: merchant loses ₹50K to a stolen card, gets a chargeback, pays penalty. (B) False positive: legit customer blocked, they leave angry, never come back, tweet about it.
**What the agent does:** Evaluates every transaction against ML risk model + merchant-defined rules. On HIGH risk: blocks and alerts merchant with explanation ("Card BIN from Nigeria, IP from VPN, first-time customer, ₹48K order. Blocked."). On MEDIUM risk: holds for 30 seconds, runs additional Secure ID device check, auto-releases if clean. On FALSE POSITIVE pattern (same customer blocked 3 times, all legit): auto-adjusts rule threshold and alerts merchant: "Your velocity limit of 5 txns/hour is too aggressive for returning customers. Recommend 10."
**Who loses money without this:** E-commerce (chargebacks), gaming (friendly fraud), high-AOV merchants (card-not-present fraud). RiskShield claims 40% fraud reduction. The agent adds the learning loop that makes it smarter over time.

### 5. Dispute Shield Agent
**Cashfree APIs:** Disputes (Accept, Get, Submit Evidence), Dispute Webhooks, Get Payment by ID, Get Order
**The pain:** Dispute notification arrives. Merchant has 7-10 days to respond. Ops team is busy. They see it on day 6. Scramble to gather evidence. Miss the deadline. Auto-lose. Or: they submit weak evidence (just an order screenshot) and lose anyway.
**What the agent does:** On dispute webhook, IMMEDIATELY pulls: transaction metadata, order details, delivery confirmation (from merchant's system via webhook), customer communication logs, IP/device data from RiskShield, Secure ID verification data if KYC was done. Assembles evidence packet. Submits via API within 24 hours. For "item not received" disputes: auto-pulls tracking data. For "unauthorized transaction": auto-pulls Secure ID face match + liveness data as evidence of authenticated customer.
**Who loses money without this:** Everyone who processes cards. Industry average: merchants lose 60-80% of disputes because they respond late or with insufficient evidence. Auto-response within 24 hours with structured evidence can flip win rate to 40-50%.

---

## CHECKOUT & CONVERSION AGENTS

### 6. One-Click Conversion Agent
**Cashfree APIs:** One Click Checkout, Token Vault (Saved Cards, Cryptogram), Get Card BIN
**The pain:** Returning customer lands on checkout. Has to re-enter card details. Or search for their UPI ID. Gets annoyed. Drops off. Merchant paid ₹200 in CAC to bring them back, then lost them at the last step.
**What the agent does:** Detects returning customer (phone number/email match). Auto-surfaces saved instruments: tokenized card (last 4 digits shown), saved UPI handle, preferred wallet. One-tap payment. If saved card is expired: auto-alerts customer + suggests adding new card. Tracks: "One-Click Checkout improved your returning customer conversion by 23% this month."
**Who loses money without this:** Subscription businesses, D2C with repeat customers, food delivery, grocery. Every extra step at checkout = 10-15% dropoff.

### 7. Cart Recovery Agent
**Cashfree APIs:** Payment Links (Create, Get, Cancel), Payment Link Webhooks
**The pain:** Customer adds items to cart. Gets to checkout. Leaves. Standard abandoned cart emails have 5-10% recovery rate. Most merchants don't even send them.
**What the agent does:** On checkout abandonment event (from merchant's system): waits 30 minutes (not instantly, that's creepy). Generates a Payment Link with the exact cart amount + a time-limited incentive (₹50 off or free shipping, configurable by merchant). Sends via WhatsApp (not email, 3x open rate in India). If first link expires unused, sends a second nudge 24 hours later with slightly better offer. If recovered: logs the conversion. Monthly report: "Recovered 847 carts worth ₹12.4L this month. Cost: ₹0.50 per WhatsApp message."
**Who loses money without this:** Every e-commerce business. Cart abandonment rate in India: 70-80%. Even 5% recovery on a ₹1Cr/month business = ₹5L recovered.

### 8. Cashfree Here Agent (Agentic Commerce)
**Cashfree APIs:** Cashfree Here (MCP), Payment Gateway, Secure ID
**The pain:** AI chatbots can recommend products but can't take payment. Customer has to leave the chat, go to a website, find the product again, and checkout. 60%+ drop off during this redirect.
**What the agent does:** Customer talks to merchant's AI assistant. Browses products. Decides to buy. Payment happens INSIDE the chat. No redirect. No new tab. UPI intent or card entry inline. If KYC required (insurance, lending): Secure ID verification inline too. Full purchase loop without leaving the conversation.
**Who loses money without this:** Any business using AI chatbots for sales (which is every business by 2027). The entire agentic commerce wave depends on in-chat payments. Cashfree Here is the rails. This agent is the intelligence.

### 9. EMI/BNPL Optimizer Agent
**Cashfree APIs:** Offers & Eligibility (Get Eligible Payment Methods, Eligible Cardless EMI, Eligible PayLater), Card EMI, BNPL
**The pain:** Customer sees ₹45,000 laptop. Can't pay full amount. Leaves. Merchant doesn't know the customer was eligible for no-cost EMI on their HDFC card. Or that Simpl would have approved them for pay-later.
**What the agent does:** On cart page (before checkout): checks customer's eligibility across ALL EMI providers (15+ card issuers), cardless EMI (FlexMoney, ZestMoney, KreditBee), and BNPL (Simpl, LazyPay, ePayLater). Surfaces the best option: "Pay ₹3,750/month x 12 months. No cost EMI on HDFC Credit Card." Shows this on the product page, not just at checkout. Tracks: "EMI surfacing increased your AOV by 34% for orders above ₹10K."
**Who loses money without this:** Electronics, furniture, education, healthcare. High-AOV merchants where the full price is a barrier. Industry data: EMI option at checkout increases conversion by 20-40% for orders above ₹5K.

---

## UPI & RECURRING PAYMENT AGENTS

### 10. UPI Autopay Lifecycle Agent
**Cashfree APIs:** Subscriptions (Create Mandate, Raise Payment, Manage Mandate, Retry), UPI Autopay
**The pain:** Merchant sets up UPI Autopay for subscription. 30% of mandates never get activated (customer ignores the approve request). Of those activated, 15% of monthly debits fail silently. Customer churns without knowing their payment failed.
**What the agent does:** Phase 1 (Activation): Customer creates mandate. Agent monitors. If not approved in 4 hours, sends WhatsApp reminder: "Your subscription is almost active. Approve in your UPI app." If not approved in 24 hours, sends a direct UPI Intent link. Phase 2 (Ongoing): On debit failure, waits until next payday window (25th-5th of month), retries. If retry fails, sends WhatsApp: "Your ₹499 subscription payment didn't go through. [Pay Now] link." Tracks mandate activation rate and suggests optimal debit timing per customer.
**Who loses money without this:** OTT, SaaS, insurance, EdTech. Silent churn from failed mandates is the #1 revenue leak for subscription businesses in India. Most don't even know it's happening.

### 11. eNACH/Physical NACH Agent
**Cashfree APIs:** Subscriptions (Physical NACH Mandate, eNACH), BAV
**The pain:** Lending companies use eNACH for EMI collection. Mandate activation takes 1-2 days. 20% fail activation (wrong bank details, unsigned mandate, bank rejection). Physical NACH is worse: paper form, courier, 2-3 week activation. Meanwhile, the first EMI date passes.
**What the agent does:** On mandate creation: validates bank account via BAV first (catches wrong account details before mandate submission). Monitors activation status. On failure: diagnoses reason (wrong IFSC, account frozen, bank not supported). Auto-falls back: eNACH failed? Try UPI Autopay. UPI failed? Generate Physical NACH form. Schedules first debit optimally (2 days after confirmed activation, on a weekday, during bank hours). Reports: "34 mandates failed activation this week. 28 auto-recovered via UPI Autopay fallback."
**Who loses money without this:** NBFCs, lending fintechs, insurance. Failed mandate = missed EMI = NPA risk. ₹1Cr loan book with 15% mandate failure rate = ₹15L at risk every collection cycle.

---

## PAYOUTS & DISBURSEMENT AGENTS

### 12. Allocation Agent (Co-Lending)
**Cashfree APIs:** Payouts (Direct Transfer, Batch Transfer), Easy Split, OneEscrow
**The pain:** NBFC does co-lending with a bank. 80/20 split. Every disbursement requires: calculate bank's share, NBFC's share, processing fee, insurance deduction, GST TDS. Finance team does this in Excel. Manual payout initiation. Reconciliation takes 3 days. RBI CLA 2025 now requires mandatory escrow + 15-day settlement.
**What the agent does:** On loan approval event: calculates exact split per RBI CLA rules. Creates OneEscrow virtual account. Routes funds: 80% to bank partner, 20% held in NBFC escrow, processing fee deducted, insurance premium routed to insurer, TDS calculated and held. Fires Cashfree Payouts for each leg. Generates reconciliation report. If settlement deadline approaching (day 12 of 15): escalation alert. Full audit trail for RBI inspection.
**Who loses money without this:** Every NBFC doing co-lending (2,000+ in India). RBI CLA 2025 is effective Jan 1, 2026. Non-compliance = penalties up to ₹250 Cr. Manual allocation = errors, delays, audit failures.

### 13. Beneficiary Verification Agent
**Cashfree APIs:** Payouts (Add Beneficiary, Get Beneficiary Details), Secure ID BAV (Sync/Async, Bulk), IFSC Verification
**The pain:** Marketplace pays out to 5,000 vendors weekly. 3% have wrong bank details. Payout fails. Vendor complains. Ops team manually fixes. Some vendors gave someone else's account (fraud). Money gone.
**What the agent does:** Before EVERY payout: verifies beneficiary bank account via BAV (name match against registered vendor name). On mismatch: holds payout, alerts ops: "Vendor Rajesh Electronics submitted account in name 'Suresh Kumar.' Name mismatch. Investigate." On IFSC change (bank merger): auto-updates IFSC via IFSC Verification API. Runs bulk BAV nightly for all active beneficiaries. Catches account closures before payout day.
**Who loses money without this:** Marketplaces, gig platforms, any business doing 100+ payouts/day. Industry data: 2-5% of payouts fail due to wrong bank details. On ₹1Cr monthly payouts = ₹2-5L stuck in failed transfers + ops cost to fix.

### 14. Payroll Autopilot Agent
**Cashfree APIs:** Payouts (Batch Transfer v2), BAV (Bulk), Get Balance
**The pain:** Every month, HR exports employee list. Finance validates bank details (manually). Runs batch transfer. 5-10 failures. Employees ping HR. HR pings finance. Finance retries manually. TDS calculation done separately. Salary slips generated separately.
**What the agent does:** On payroll trigger (1st or last day of month): pulls employee list from HR system. Runs bulk BAV to validate all accounts. Flags any changes ("Employee #234's account IFSC changed due to bank merger. Updated."). Checks Payouts balance ("Balance ₹45L. Payroll requirement ₹42L. Sufficient."). Fires batch transfer. On individual failure: auto-retries once. If still fails: alerts HR with specific reason ("Employee #567: account frozen by bank."). Generates TDS report per employee for compliance.
**Who loses money without this:** Every company with 50+ employees. Payroll day is chaos for finance teams. Even one failed salary = unhappy employee + HR escalation.

### 15. Instant Refund Agent
**Cashfree APIs:** Refunds (Create by Order/Payment ID), Cashgram (Create, Get Status), Refund Webhooks
**The pain:** Customer returns a product. Refund takes 5-7 days to process. Customer calls support 3 times. Leaves a 1-star review. Or worse: merchant doesn't have customer's bank details (COD order), so they literally can't refund.
**What the agent does:** On refund trigger: checks if instant refund is available (Cashfree supports instant refunds for eligible transactions). If yes, fires instantly. Customer gets money in seconds. If not eligible (COD, bank transfer): auto-generates Cashgram link, sends via WhatsApp: "Your refund of ₹2,499 is ready. Claim it here: [link]." If Cashgram unclaimed after 48 hours: sends reminder. Tracks: "Average refund TAT reduced from 5.2 days to 4 hours. Customer complaints about refunds dropped 78%."
**Who loses money without this:** Every e-commerce business. Slow refunds = negative reviews = lower conversion = higher CAC. Cashgram solves the "I don't have their bank details" problem that kills COD refunds.

### 16. Marketplace Split Agent
**Cashfree APIs:** Easy Split (Create Vendor, Split After Payment, On-Demand Transfer, Vendor Reconciliation)
**The pain:** Multi-vendor marketplace. Customer pays ₹5,000 for items from 3 vendors. Platform takes 15% commission. GST TDS needs to be deducted. Each vendor has different settlement terms (Vendor A: instant, Vendor B: T+2, Vendor C: weekly). Finance team does this in spreadsheets. Reconciliation is a nightmare.
**What the agent does:** On order completion: auto-calculates split per vendor. Deducts platform commission (15% = ₹750). Calculates GST TDS per vendor. Routes to each vendor on their configured schedule (instant/T+2/weekly). Generates vendor-level reconciliation. Sends each vendor a WhatsApp settlement report: "Order #4521: ₹1,200 total, ₹180 commission, ₹1,020 settled to your account." Monthly vendor reconciliation report auto-generated.
**Who loses money without this:** B2B marketplaces, multi-vendor e-commerce, aggregator platforms. Manual split calculations = errors. One wrong split = vendor dispute + platform reputation damage.

---

## VERIFICATION & COMPLIANCE AGENTS

### 17. KYC Onboarding Agent
**Cashfree APIs:** Secure ID (PAN 360, Aadhaar via DigiLocker, SmartOCR, FaceMatch, Liveness, BAV, CKYC, 1-Click Onboarding)
**The pain:** Bank wants to onboard a new customer. Needs: PAN check, Aadhaar verification, face match, liveness, bank account verification, CKYC lookup/upload. Today: 6 separate API integrations. 6 vendor dashboards. If any step fails, customer starts over. Onboarding takes 3-5 days. 40% drop off during KYC.
**What the agent does:** Customer submits phone number. Agent runs 1-Click Onboarding (Mobile360): pulls 15+ data points from bureau, EPFO, telco, PAN in one shot. If data sufficient for low-risk: auto-approves without Video KYC. If medium-risk: triggers PAN + Aadhaar + FaceMatch + Liveness in sequence. If any step fails: retries with fallback (DigiLocker fails? Try SmartOCR). If high-risk: routes to Video KYC. All results logged as single audit trail. Customer never sees the complexity. Onboarding: 30 seconds for low-risk, 5 minutes for standard.
**Who loses money without this:** Every regulated entity. 40% KYC drop-off = 40% of potential customers lost. BCG data: 60% of onboarding costs are KYC. Automating this is the single biggest ROI for any bank/NBFC.

### 18. Mule Detection Agent
**Cashfree APIs:** Secure ID (BAV, FaceMatch, Device Intelligence), RiskShield, Payouts Intelligence
**The pain:** 90% of mule accounts pass standard KYC because the identity is real. The intent is fraudulent. Bank opens the account. Mule deposits ₹5L on day 1. Withdraws on day 3. Account used for money laundering. Bank gets RBI notice.
**What the agent does:** Two phases. Phase 1 (Onboarding): Runs standard KYC + additional signals: device velocity (same device, 5 applications today?), geographic mismatch (KYC address: Mumbai, login: Assam, 1,500km gap), SIM age (<30 days = red flag), Cashfree transaction intelligence (this phone number linked to 3 disputed transactions on other merchants). Phase 2 (Post-activation): T+1: checks first deposit pattern. T+7: checks withdrawal velocity. T+30: compares balance vs transaction size (₹1L average balance but ₹6-7L moving through = profile mismatch). On red flag: auto-freezes account, creates case for investigation, reports to compliance.
**Who loses money without this:** Banks. RBI mandate for mule detection. HDFC Bank's "Zero Block Account" philosophy. 62 NBFCs penalized in 2023-2025. This is the fastest-growing compliance segment.

### 19. Re-KYC Enforcer Agent
**Cashfree APIs:** Secure ID (all verification APIs), E-Sign
**The pain:** RBI requires periodic KYC refresh. Bank has 50L customers. 20% have KYC older than 2 years. Nobody's tracking it. Compliance team manually pulls lists. Sends letters. 5% response rate. RBI audit finds gaps. Fine.
**What the agent does:** Scheduled weekly: scans customer base for KYC expiry (approaching 2-year mark). 60 days before expiry: auto-sends WhatsApp with re-KYC link (Cashfree Secure ID journey). 30 days: reminder + branch walk-in option. 7 days: escalation to relationship manager. On expiry: auto-restricts account per RBI guidelines. On re-KYC completion: updates records, uploads to CKYC. Dashboard: "12,400 customers due for re-KYC this quarter. 8,200 completed. 4,200 pending. 340 accounts restricted."
**Who loses money without this:** Every bank and NBFC. Non-compliance = RBI penalties. Manual re-KYC = ₹500+ per customer in ops cost. Automated = ₹5 per customer.

### 20. Credit Pre-Screen Agent
**Cashfree APIs:** Account Aggregator (Consent, Financial Info), Secure ID (1-Click Onboarding, PAN 360), Payouts (Lend)
**The pain:** Customer applies for a ₹50K loan. Lending company runs full KYC (₹50-100 per customer). Customer fails credit check. ₹50-100 wasted. At 10,000 applications/day with 60% rejection rate: ₹3-6L/day wasted on KYC for people who would fail anyway.
**What the agent does:** BEFORE any paid verification: runs 1-Click Onboarding (mobile number only, ₹5-10 cost). Gets bureau score, employment status, basic identity. If pre-screen fails (score too low, no employment): reject immediately. ₹5 spent, not ₹100. If pre-screen passes: proceed to full KYC. On approval + KYC complete + e-sign done: fires Payouts Lend API for instant disbursement. Reports: "Pre-screening rejected 4,200 applications before KYC. Saved ₹4.2L in verification costs this week."
**Who loses money without this:** Digital lenders, BNPL providers, NBFCs with high application volumes. The math is simple: ₹5 pre-screen vs ₹100 full KYC. At scale, this saves crores.

---

## RECONCILIATION & INTELLIGENCE AGENTS

### 21. Inward Reconciliation Agent
**Cashfree APIs:** Virtual Banking (Create VBA, Get VBA Payments, Get Payments by UTR), Settlement Reconciliation
**The pain:** B2B company receives payments via bank transfer (NEFT/IMPS/RTGS). 200 payments/day. Finance team downloads bank statement. Manually matches each payment to an invoice. Takes 2-3 hours daily. Some payments can't be matched (wrong reference number). Sit unmatched for weeks.
**What the agent does:** Creates a unique Virtual Bank Account per customer/invoice. Customer pays to their specific VBA. Agent auto-matches by UTR: "Payment ₹2,45,000 received from ABC Corp. Matched to Invoice #INV-4521. Marked as paid." Unmatched payments: agent alerts finance with details ("₹50,000 received to VBA-general. No matching invoice. Sender: 'XYZ PVT LTD'. Possible match: Invoice #INV-4498 for ₹50,000 from XYZ Pvt Ltd." Finance confirms with one click.
**Who loses money without this:** B2B businesses, SaaS companies, educational institutions. Manual reconciliation = 2-3 hours/day of a finance person's time. At ₹50K/month salary = ₹25K/month just for reconciliation. Plus: unmatched payments = delayed revenue recognition.

### 22. CashRadar (Cashflow Forecast) Agent
**Cashfree APIs:** Settlements (Get, Get for Order), Payouts (Get Balance), Easy Split (Get On-Demand Balance), Reconciliation
**The pain:** Friday afternoon. Payroll is Monday. CFO asks: "Will we have enough cash?" Nobody knows. Settlements are T+1/T+2. Some are pending. Payouts committed for vendor payments. Easy Split vendor settlements due. The actual cash position is a mystery.
**What the agent does:** Daily at 8am: calculates net cash position. Inflows: pending settlements (by T+date), expected Payment Link collections, subscription debits due. Outflows: scheduled payouts, vendor settlements, payroll commitment, tax payments. Projects 7-day rolling forecast. Alerts: "Cash balance will drop below ₹5L on Wednesday. Payroll of ₹42L due Monday. You need ₹37L in settlements to clear by Friday." If projected shortfall: suggests marking orders for faster settlement.
**Who loses money without this:** Every business. Cash flow is the #1 killer of small businesses. Most merchants have NO visibility into their T+2 position. They're flying blind.

### 23. Daily Ledger Agent
**Cashfree APIs:** Settlements (Get), Reconciliation, Easy Split (Vendor Reconciliation)
**The pain:** Merchant logs into dashboard every morning. Checks yesterday's settlements. Cross-references with orders. Tries to figure out: "How much did I actually make yesterday after fees?" This takes 30 minutes. Every day.
**What the agent does:** Every morning at 8am: pulls yesterday's settlement batch. Formats: gross collected, Cashfree fees, GST on fees, net settled, pending for tomorrow, disputes held. If merchant uses Easy Split: includes vendor settlement summary. Delivers via WhatsApp or Slack. No dashboard login needed. One glance: "Yesterday: ₹8.4L collected. ₹12K fees. ₹8.28L settled. ₹2.1L pending T+2."
**Who loses money without this:** Not about losing money. About saving 30 minutes every morning for every merchant. At 8L+ merchants, that's 4L hours/day of dashboard-staring eliminated.

---

## RTO & COD AGENTS

### 24. RTO Guard Agent
**Cashfree APIs:** Risk Management (Submit Risk Details), Secure ID (Device Intelligence, BAV), Payment Links
**The pain:** D2C brand ships 1,000 COD orders/day. 25% come back as RTO (Return to Origin). Each RTO costs ₹150-300 in shipping + packaging + restocking. At 250 RTOs/day: ₹37,500-75,000 DAILY. That's ₹11-22L/month in pure waste.
**What the agent does:** On COD order creation: scores the order. Signals: customer's past RTO rate, PIN code RTO rate (some PIN codes have 60% RTO), order value vs customer's average, device fingerprint (same device, 10 COD orders this week?), address quality (incomplete address = high RTO). High risk (score >70): auto-converts to prepaid. Generates Payment Link with ₹50 discount for prepaid. Sends via WhatsApp: "Pay now and save ₹50! [Pay ₹2,450 instead of ₹2,500]." If customer pays: COD converted. If not: flags for ops review before shipping. Reports: "Blocked 89 high-risk COD orders this week. Converted 34 to prepaid. Estimated RTO cost avoided: ₹2.1L."
**Who loses money without this:** Every D2C brand, quick commerce, fashion e-commerce. RTO is the silent killer of Indian e-commerce margins. 20-30% RTO rate is industry average. Reducing by even 5% = crores saved annually for mid-size merchants.

### 25. RTO Insights Agent
**Cashfree APIs:** Settlements Reconciliation, Refunds (Get for Order)
**The pain:** Merchant knows RTO is a problem. Doesn't know WHY. Is it specific PIN codes? Specific products? Specific customer segments? Specific days of the week? Without data, they can't fix it.
**What the agent does:** Weekly scheduled report. Pulls all refund + return data. Cross-references by: PIN code (top 10 RTO PIN codes), product SKU (which products get returned most), customer segment (first-time vs repeat), order value bracket, day of week, payment method (COD vs prepaid RTO rates). Delivers actionable report: "Your top 3 RTO PIN codes are 110085, 400069, 560034. Consider prepaid-only for these. Product 'Blue Denim Jacket L' has 45% return rate, possible sizing issue."
**Who loses money without this:** Same as RTO Guard. But this is the intelligence layer that tells you WHERE to fix, not just what to block.

---

## SUBSCRIPTION & MANDATE AGENTS

### 26. Mandate Reviver Agent
**Cashfree APIs:** Subscriptions (Raise Payment, Retry, Manage, Fetch Payments for Mandate), Cashgram
**The pain:** SaaS company has 10,000 subscribers. 500 payments fail every month. Most customers don't even know. They lose access on day 15. Call support. "Why was I deactivated?" Silent churn.
**What the agent does:** On payment failure: waits 24 hours (bank might process overnight). Retries at optimal time (morning of next business day, when bank liquidity is highest). If retry fails: sends WhatsApp: "Your ₹999/month subscription payment didn't go through. [Retry Now] or [Update Payment Method]." If all retries fail: generates Cashgram link as last resort ("Can't charge your account? Claim your subscription credit here."). Tracks: "Recovered 234 of 500 failed subscription payments this month. Revenue saved: ₹2.3L."
**Who loses money without this:** Every subscription business. Industry data: 20-30% of churn is involuntary (failed payments, not conscious cancellation). Solving this is literally free revenue.

---

## ESCROW & TRUST AGENTS

### 27. Escrow Orchestrator Agent
**Cashfree APIs:** OneEscrow (Create VA, Allocate Funds, Transfer, Get Statement)
**The pain:** Freelance platform. Client pays ₹50,000 for a project. Money should be held until milestone is delivered. Today: manual escrow. Client pays into platform's bank account. Platform manually tracks milestones. Manually releases funds. Reconciliation is a nightmare. If platform goes bankrupt, money is mixed with operating funds.
**What the agent does:** On project creation: creates dedicated OneEscrow virtual account. Client pays into escrow VA (not platform's main account). Agent monitors milestone events from platform. On milestone approved by client: releases proportional funds to freelancer via Payouts. On dispute: holds funds, routes to dispute resolution. On project cancellation: auto-refunds client from escrow. Full audit trail. Money never touches platform's operating account.
**Who loses money without this:** Freelance platforms, real estate (advance payments), B2B contracts, event management. Trust is the product. Without segregated escrow, platforms are one bad actor away from a regulatory nightmare.

---

## INTERNATIONAL & CROSS-BORDER AGENTS

### 28. CrossBorder Settlement Agent
**Cashfree APIs:** International Payments (ICA Settlement, Reconciliation, Payment Verification), Global Collections
**The pain:** SaaS company sells to US customers. Payments come in USD. Settle in INR. FX rate fluctuates. Merchant doesn't know what rate they'll get until settlement hits. Can't plan pricing. Some settlements get flagged for additional verification (RBI purpose code compliance). Delays.
**What the agent does:** On international payment received: tracks expected settlement date and estimated INR amount at current FX rate. Alerts if rate moves more than 2% before settlement: "Your $5,000 settlement would be ₹4.18L at current rate. Yesterday it was ₹4.25L. Consider marking for faster settlement." Auto-uploads payment verification details. Monitors settlement status. On delay: diagnoses (purpose code issue? verification pending?) and alerts with action needed.
**Who loses money without this:** SaaS, EdTech, freelancers with international clients. FX rate uncertainty = pricing uncertainty = margin risk. Even 2% FX swing on ₹1Cr monthly international revenue = ₹2L impact.

### 29. International Checkout Agent
**Cashfree APIs:** International Payments (140+ currencies), Apple Pay, Get Card BIN Details, Dynamic Currency Conversion
**The pain:** Indian merchant sells globally. Customer in US sees price in INR. Confused. Drops off. Or: customer tries Apple Pay. Not supported. Drops off. Or: international card gets declined because merchant's gateway doesn't support that BIN.
**What the agent does:** Detects international card BIN. Auto-shows price in customer's local currency (DCC). Enables Apple Pay for supported card types (reduces dropoff up to 75% for international transactions). If card declines: diagnoses BIN type, suggests alternative payment method. Reports: "Your international conversion rate improved 18% after enabling DCC + Apple Pay."
**Who loses money without this:** Export businesses, SaaS with global customers, D2C brands selling internationally. International card decline rates are 2-3x domestic. Every improvement matters.

---

## PLATFORM & OPERATIONS AGENTS

### 30. Sub-Merchant Activator Agent
**Cashfree APIs:** Platforms (Create Merchant, Get Status, Onboarding Links, Upload Documents, KYC Webhooks)
**The pain:** Payment aggregator onboards 100 sub-merchants/month. Each needs: business verification, KYC documents, bank account verification, RBI compliance check. Onboarding team manually reviews each. Average activation time: 7-10 days. Sub-merchants get frustrated. Some abandon the process.
**What the agent does:** On new sub-merchant registration: creates embeddable onboarding link. Monitors document uploads. Auto-validates: PAN matches business name? Bank account verified via BAV? GST registration valid? All docs clear? If all checks pass: auto-activates. If issue found: specific alert to onboarding team: "Merchant #234: PAN name 'ABC Traders' doesn't match bank account name 'ABC Trading Co.' Needs manual review." Tracks: "56 of 100 merchants auto-activated this month. Average activation time: 2 hours (down from 8 days)."
**Who loses money without this:** Payment aggregators, platforms, marketplaces. Slow onboarding = lost merchants. Every day a sub-merchant waits = potential revenue they're processing through a competitor.

### 31. Field Ops Monitor Agent
**Cashfree APIs:** SoftPOS (Create Terminal, Get Transactions, Terminal Status, QR Codes)
**The pain:** Chain of 50 retail stores uses SoftPOS. Head office has no visibility: Which terminals are active? Which stores processed zero transactions today? Is the QR code still working? They find out problems when a store manager calls.
**What the agent does:** Daily scan: checks all terminal statuses. Flags dormant terminals (zero transactions in 48 hours). Alerts: "Store #12 Indiranagar has 0 transactions since Tuesday. Terminal status: active. Possible issue: QR code not displayed or staff not trained." Weekly: performance ranking by store. Monthly: terminal utilization report. On new SoftPOS deployment: auto-onboards VPA, generates QR code, sends to store manager.
**Who loses money without this:** Retail chains, restaurant chains, franchise businesses using SoftPOS. A dormant terminal = lost sales that went to cash (no tracking, no data, no loyalty).

### 32. Wallet Lifecycle Agent
**Cashfree APIs:** PPI Wallet (Create, Credit, Debit, KYC, Eligibility, Statement, Close)
**The pain:** Fintech launches a wallet product. Customer does min-KYC. Uses wallet for small transactions. Hits the ₹10,000 limit. Can't transact more. Doesn't know they need to upgrade KYC. Abandons the wallet.
**What the agent does:** Monitors wallet usage patterns. When balance approaches 80% of min-KYC limit: sends upgrade nudge: "You've used ₹8,000 of your ₹10,000 wallet limit. Complete full KYC in 2 minutes to unlock unlimited transactions. [Upgrade Now]." Links to Secure ID full-KYC flow. On dormancy (no transaction in 30 days): sends re-engagement: "Your wallet has ₹450. Use it or withdraw." On wallet closure request: handles fund repatriation (remaining balance back to bank account).
**Who loses money without this:** Any business with a PPI wallet (fintech, gaming, food delivery). 60-70% of min-KYC wallets never upgrade to full KYC. That's 60-70% of customers capped at ₹10K. Nudging them to upgrade = unlock lifetime value.

### 33. Bill Collection Agent
**Cashfree APIs:** BBPS (Transaction Status), Payment Links (Create)
**The pain:** Insurance company has 1L policyholders. Premium due dates spread across the month. Manual reminder calls cost ₹15-20 per call. 30% of premiums are paid late. Late payments = policy lapse risk = regulatory issue + lost customer.
**What the agent does:** 7 days before due: WhatsApp reminder with Payment Link: "Your premium of ₹12,500 is due on March 30. [Pay Now]." 1 day before: second reminder. On due date: if unpaid, sends "Grace period: 15 days remaining." 3 days after: escalation to agent. On payment received: confirms via BBPS transaction status, updates policy system. Reports: "On-time premium collection improved from 70% to 89% this quarter."
**Who loses money without this:** Insurance, utilities, educational institutions, housing societies. Late collection = cash flow gap + ops cost of chasing payments.

### 34. E-Sign Closer Agent
**Cashfree APIs:** E-Sign (Upload Document, Create Request, Get Status, Webhooks)
**The pain:** Loan approved. KYC done. Agreement generated. Sent for e-sign. Customer opens email 3 days later. By then, they've taken a loan from someone else. Or: customer starts e-sign, gets distracted, never completes. Loan stuck in "pending documentation."
**What the agent does:** On loan approval: immediately triggers e-sign request. Sends agreement via WhatsApp (not just email). If not signed in 2 hours: WhatsApp reminder: "Your loan of ₹2L is approved! Sign the agreement to receive funds. Takes 30 seconds. [Sign Now]." On signature: immediately triggers disbursement via Payouts. Reports: "Agreement-to-disbursement time reduced from 3.2 days to 4 hours."
**Who loses money without this:** Lending companies. Every hour between approval and disbursement = risk of losing the customer to a competitor. In digital lending, speed IS the product.

---

## THE FULL MAP

| # | Agent | Cashfree Product(s) | Real Pain Solved |
|---|-------|-------------------|-----------------|
| 1 | Payment Failure Diagnostics | PG, Incidents | "Why did the payment fail?" answered in real-time |
| 2 | Smart Retry | PG, Payment Links, Subscriptions | Recovers "failed" payments that actually succeed with retry |
| 3 | FlowWise Routing | FlowWise (all routing types) | Auto-routes to best gateway. Saves MDR. Increases success rate. |
| 4 | RiskShield Fraud | RiskShield, Secure ID | Blocks fraud without blocking good customers |
| 5 | Dispute Shield | Disputes API | Auto-submits evidence within 24 hours. Flips dispute win rate. |
| 6 | One-Click Conversion | One Click Checkout, Token Vault | Returning customers pay in one tap. 23% conversion lift. |
| 7 | Cart Recovery | Payment Links | Recovers 5-10% of abandoned carts via WhatsApp. |
| 8 | Cashfree Here | Cashfree Here (MCP), PG, Secure ID | In-chat payments. No redirect. No drop-off. |
| 9 | EMI/BNPL Optimizer | Offers, Card EMI, Cardless EMI, BNPL | Shows best EMI/BNPL option. Increases AOV 34%. |
| 10 | UPI Autopay Lifecycle | Subscriptions, UPI Autopay | Fixes mandate activation failures. Reduces silent churn. |
| 11 | eNACH/NACH | Subscriptions, eNACH, BAV | Mandate activation, failure recovery, auto-fallback. |
| 12 | Allocation (Co-Lending) | Payouts, Easy Split, OneEscrow | Co-lending splits. RBI CLA 2025 compliant. |
| 13 | Beneficiary Verification | Payouts, BAV, IFSC | Validates accounts before payout. Catches fraud + errors. |
| 14 | Payroll Autopilot | Payouts Batch, BAV | End-to-end payroll. Validates accounts. Handles failures. |
| 15 | Instant Refund | Refunds, Cashgram | Instant refund for online. Cashgram for COD. |
| 16 | Marketplace Split | Easy Split (all APIs) | Auto-calculates vendor splits + TDS. Reconciliation included. |
| 17 | KYC Onboarding | Secure ID (full suite) | 6 vendor integrations become one config. 30-second onboarding. |
| 18 | Mule Detection | Secure ID, RiskShield, Payouts Intel | Catches mule accounts that pass standard KYC. |
| 19 | Re-KYC Enforcer | Secure ID, E-Sign | Auto-triggers re-KYC before expiry. Avoids RBI penalties. |
| 20 | Credit Pre-Screen | Account Aggregator, 1-Click, Payouts Lend | Rejects bad applicants at ₹5, not ₹100. Saves crores. |
| 21 | Inward Reconciliation | Virtual Banking (VBA) | Auto-matches payments to invoices. Eliminates manual reconciliation. |
| 22 | CashRadar (Forecast) | Settlements, Payouts Balance, Easy Split | 7-day cash forecast. Alerts before shortfall. |
| 23 | Daily Ledger | Settlements, Reconciliation | Morning WhatsApp: yesterday's numbers. No dashboard needed. |
| 24 | RTO Guard | Risk Management, Secure ID, Payment Links | Blocks high-risk COD. Converts to prepaid. Saves ₹2L+/week. |
| 25 | RTO Insights | Settlements, Refunds | Weekly report: WHY returns happen. Pincode, product, customer. |
| 26 | Mandate Reviver | Subscriptions (all APIs), Cashgram | Recovers failed subscription payments. 40-50% recovery rate. |
| 27 | Escrow Orchestrator | OneEscrow (all APIs), Payouts | Automated escrow. Hold until milestone. Release on approval. |
| 28 | CrossBorder Settlement | International Payments, Global Collections | FX tracking, settlement monitoring, verification auto-upload. |
| 29 | International Checkout | International (140+ currencies), Apple Pay, BIN | DCC, Apple Pay, BIN-aware routing. Reduces intl declines. |
| 30 | Sub-Merchant Activator | Platforms (all APIs) | Auto-activates sub-merchants. 8 days to 2 hours. |
| 31 | Field Ops Monitor | SoftPOS (all APIs) | Terminal health monitoring. Flags dormant devices. |
| 32 | Wallet Lifecycle | PPI Wallet (all APIs) | Min-KYC to full-KYC upgrade nudges. Reactivation. Closure. |
| 33 | Bill Collection | BBPS, Payment Links | Premium/bill reminders. On-time collection 70% to 89%. |
| 34 | E-Sign Closer | E-Sign (all APIs), Payouts | Agreement signed in hours, not days. Auto-triggers disbursement. |

---

## DECISION AUTOMATION AGENTS (Taktile-Inspired)

These agents address the decision intelligence layer that platforms like Taktile ($100M+ funding) solve for banks. But Taktile is a standalone platform. These agents are native to Cashfree, with direct execution capability.

### 35. Credit Strategy Agent
**Cashfree APIs:** Account Aggregator, Secure ID (1-Click, PAN 360), Payouts (Lend)
**The pain (Taktile validates this):** Lending company has a credit policy. Written in a document. Implemented in code by engineers. Every time the risk team wants to change a threshold (CIBIL cutoff from 680 to 700), they file a Jira ticket. Wait 2 weeks for a sprint. Deploy. A/B test manually. Taktile charges ₹50L+/year to solve this.
**What the agent does:** Credit rules as config, not code. Risk team changes thresholds in a visual builder. Agent runs champion/challenger: 80% of applications go through current rules (champion), 20% through new rules (challenger). After 1,000 decisions: auto-reports: "Challenger rules rejected 3% more applicants but reduced default rate by 22%. Recommend promoting to champion." On approval: fires Payouts Lend for disbursement.
**Who loses money without this:** Every lender iterating on credit policy. 2-week engineering cycle per rule change = 2 weeks of sub-optimal decisions = lakhs in bad loans or lost good customers.

### 36. AML Case Manager Agent
**Cashfree APIs:** Secure ID (AML/PEP/Sanctions Check, Adverse Media, Network Analysis), Disputes API
**The pain (Taktile validates this):** Bank's AML system generates 500 alerts/day. 85% are false positives. Compliance team manually reviews each. Takes 45 minutes per case. That's 375 hours/day of analyst time on false positives. Taktile claims 75% false positive reduction.
**What the agent does:** On AML alert: auto-enriches with additional data (Secure ID watchlist re-check, transaction history analysis, network graph). Auto-classifies: TRUE POSITIVE (escalate immediately), LIKELY FALSE POSITIVE (auto-close with explanation), NEEDS REVIEW (route to analyst with pre-compiled evidence). For auto-closed cases: generates audit-ready justification. For escalated cases: pre-packages evidence for SAR filing. Reports: "375 alerts this week. 289 auto-closed as false positive (with audit trail). 52 escalated. 34 need human review. Analyst time saved: 216 hours."
**Who loses money without this:** Every bank and NBFC with an AML obligation. Compliance analyst salaries are ₹8-15L/year. If you can reduce their alert volume by 75%, the math is obvious.

### 37. Dynamic Pricing/Offer Agent
**Cashfree APIs:** Offers (Create, Get Eligible), Payment Links, Subscriptions
**The pain:** Merchant runs promotions manually. "10% off for all customers this weekend." But some customers would have paid full price. And some need 15% to convert. One-size-fits-all promotions leave money on the table.
**What the agent does:** On checkout or cart page: evaluates customer segment (new vs returning, cart value, past purchase frequency, payment method preference). Determines optimal offer: new customer with ₹5K cart gets "₹500 off via HDFC card" (bank-funded offer, zero cost to merchant). Returning customer gets "free shipping" (lower cost, still effective). High-value customer gets nothing (they'd buy anyway). Creates offer via Offers API. Reports: "Dynamic offers saved ₹2.3L in unnecessary discounts this month while maintaining conversion rate."
**Who loses money without this:** E-commerce, D2C. Blanket discounts are the most expensive way to grow. Dynamic offers based on willingness-to-pay = same conversion at lower cost.

### 38. Continuous Risk Monitoring Agent
**Cashfree APIs:** Secure ID (all verification APIs, AML/PEP refresh), RiskShield, Payouts Intelligence
**The pain (Taktile validates this):** Bank onboards a customer. Runs KYC. Customer is clean. 6 months later: customer appears on a sanctions list. Bank doesn't know until the next annual review. By then, they've processed ₹2Cr in transactions for a sanctioned entity.
**What the agent does:** Runs daily batch against updated databases: OFAC SDN (daily updates), UN sanctions, PEP lists, MuleHunter.ai (real-time), adverse media (weekly crawl). On new match: instant alert to compliance team with case details. On customer behavior change: dynamic trust score adjustment ("Customer's average transaction was ₹50K/month. This month: ₹12L. Score dropped from 85 to 45. Flagged for review."). On no issues: monthly summary: "10L customers monitored. 23 new flags. 4 confirmed issues. 19 false positives auto-cleared."
**Who loses money without this:** Every regulated entity. PMLA requires continuous monitoring. Not doing it = regulatory violation. Doing it manually = impossible at scale.

**38 agents. Every Cashfree product covered. Every agent solves a pain that costs real money today.**

---

## Count by Lifecycle Stage

| Stage | Agents | Razorpay Has? |
|-------|--------|--------------|
| **Pre-transaction** (verify, comply, onboard) | 7 agents (#17-20, #30, #32, #33) | Zero |
| **During transaction** (checkout, routing, risk) | 9 agents (#1-6, #8-9, #24) | 3 partial (cart, RTO) |
| **Post-transaction** (settle, reconcile, disburse) | 12 agents (#7, #10-16, #21-23, #25-26) | 5 partial |
| **Continuous** (monitor, detect, enforce) | 6 agents (#18-19, #27-29, #31, #34) | Zero |
| **Decision automation** (Taktile-class) | 4 agents (#35-38) | Zero |
| **Total** | **38** | **8** |

---

## The Bottom Line

Razorpay Agent Studio: 8 agents. Post-transaction intelligence. Proprietary. No community.

Cashfree (Clawpay + AI Studio): 38 agents. Full lifecycle. Open-core engine. 39 products covered. Every agent solves a pain that costs real money today.

**Razorpay has a product. Cashfree would have a movement.**
