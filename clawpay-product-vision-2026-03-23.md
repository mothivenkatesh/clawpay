# Clawpay — Product Vision Document

**The Open-Core Financial Operations Engine**
*Inspired by OpenClaw. Built for Money Movement.*

**Date:** March 23, 2026
**Author:** Sagar Mothiram | PMM, Cashfree Payments
**Status:** Draft v1.0 — For WTFraud Community Feedback

---

## 1. The Inspiration: What OpenClaw Proved

### The Fastest-Growing Open Source Project in History

OpenClaw (formerly Clawdbot, then Moltbot) is a free, open-source autonomous AI agent created by Austrian developer Peter Steinberger. In early 2026, it became the fastest-growing GitHub repository ever:

| Metric | Number |
|--------|--------|
| GitHub Stars | 250,000+ (surpassed React's 10-year record in ~100 days) |
| Stars in first 72 hours (viral week) | 60,000 |
| Weekly Visitors | 2M+ at peak |
| GitHub Forks | 47,700+ |
| Contributors | 1,075 |
| Community Skills | 13,000+ on ClawHub marketplace |
| License | MIT (fully open, commercially usable) |

Steinberger spent 13 years building PSPDFKit, sold it, retired to Madrid, got bored, and built OpenClaw in 3 months by having conversations with an AI agent. He later joined OpenAI in February 2026, with Sam Altman calling him "a genius with amazing ideas about the future of very smart agents."

**What the biggest names said:**
- **Jensen Huang (Nvidia):** "Probably the single most important release of software, you know, probably ever."
- **Andrej Karpathy:** Called Moltbook (the AI-agent social network spawned by OpenClaw) "the most incredible sci-fi takeoff-adjacent thing I have seen recently."
- **Sam Altman:** Hired Steinberger, calling him "a genius with amazing ideas about the future of very smart agents interacting with each other." OpenClaw transitioned to an independent foundation with community governance.

### Why OpenClaw Won — The 5 Principles

1. **Local-First / Self-Hostable** — Docker-based, runs on your infra, your data never leaves
2. **Open-Core MIT License** — Free forever, fork it, modify it, build commercially on top
3. **Community Skills Marketplace** — Anyone can contribute modular capabilities
4. **Autonomous Execution** — Fires on schedule, not just on trigger. Set it and forget it
5. **Composable Architecture** — Grab what you need, leave what you don't

**The three-word manifesto:** *"Your assistant. Your data. Your rules."*

### What OpenClaw Got Wrong — Lessons for Clawpay

OpenClaw's rapid growth exposed serious vulnerabilities that any financial system must avoid:

| Problem | Scale | Lesson for Clawpay |
|---------|-------|-------------------|
| **ClawHavoc Supply Chain Attack** | 1,184 malicious skill packages identified across ClawHub, attributed to 12 author IDs | Every community template touching money needs cryptographic signing + sandbox simulation before approval |
| **Malicious Skills Ratio** | ~11.9% of all skills were malicious at initial audit (341 of 2,857); Bitdefender placed it at ~20% | Template marketplace cannot be open-upload for financial workflows |
| **Exposed Instances** | 135,000+ OpenClaw instances found exposed on public internet with insecure defaults | Secure-by-default configuration is non-negotiable |
| **Memory Poisoning** | Context compression deleted safety instructions, causing an "inbox-wipe incident" | Financial rule execution needs immutable guardrails outside compressible context |
| **60+ CVEs Disclosed** | Including CVSS 8.8 and CVSS 9.9 critical vulnerabilities | Security-first architecture from day one, not bolted on later |
| **Runaway API Costs** | Power users spending $100–$700+/month on LLM calls | Clawpay's cost = transaction fees (scales with customer revenue, not usage volume) — fundamentally better unit economics |

**The crucial distinction:** OpenClaw's marketplace vulnerability risks data theft. In fintech, a compromised rule redirects money. The security bar is exponentially higher.

### The Anthropic vs. OpenAI Lesson — How You Treat Builders Matters

OpenClaw was originally called "Clawdbot" — built entirely on Anthropic's Claude API. It was functionally a billboard for Anthropic's technology. Anthropic's response was to send trademark lawyers, forcing two name changes in a week (Clawdbot → Moltbot → OpenClaw).

OpenAI's response: gave Steinberger a phone number, helped integrate their subscriptions, open-sourced their competing tool, and hired him.

**Lesson for Clawpay:** If built on Cashfree primitives, the relationship must be formal, commercial, and mutually beneficial from day one — not a side project that creates channel conflict.

---

## 2. The Market Need: Why BFSI Needs Its Own OpenClaw

### The $6.9B Problem

The global Business Rules Management System (BRMS) market sits within a broader Decision Management market valued at **$6.92 billion in 2025**, projected to reach **$19.34 billion by 2032** (15.8% CAGR).

| Market Segment | 2023 Value | 2025E Value | 2032E Value | CAGR |
|---------------|-----------|-----------|-----------|------|
| Global BRMS | $1.42B | ~$1.8B | $3.35B | 9.2–11.8% |
| Decision Management (incl. BRE) | — | $6.92B | $19.34B | 15.8% |
| India BPM Market | — | $1.70B (2026E) | — | — |
| Asia Pacific BRMS | — | — | — | 10.7–11.18% |

**BFSI is the dominant buyer** — accounting for 18–26% of global BRMS revenue, with the highest growth CAGR of 20.1% in BPM adoption.

### India's Digital Lending Explosion

India's fintech market is projected to reach **$2.1 trillion by 2030**, with digital lending capturing **53% of revenue ($133B)**.

Key data points from KPMG and BCG reports (2025):

- **Lending + Payments = 60% of all fintech funding** in India (KPMG India, Oct 2025)
- Only **2–3% of global deposit and lending revenue** has been penetrated by fintechs (BCG/QED, June 2025) — massive white space
- A **$280 billion opportunity** remains for private credit funds to acquire fintech-originated loans
- Fintechs growing **3x faster than incumbents** leveraging digital distribution and AI
- Bajaj Finance reported **INR 150 Cr/year savings** using GenAI bots
- Mid-sized banks saw **34–36% drop in credit disbursement costs** from AI adoption
- **60% of onboarding costs** are spent on KYC, credit checks, and regulatory approvals (BCG)
- **64% of banks** report onboarding-linked revenue loss — customers drop off during fragmented KYC
- **71% of banks globally** still run on legacy core banking systems; **65% lack real-time capabilities** (IDC, 2024)
- **~45% of 2024 private fintech funding** went to BFSI SaaS/infrastructure — the hottest investment category
- Banks implementing workflow orchestration report **40–60% TAT reduction** and **200% productivity gains**
- Loan processing time drops from **14 days to 3 days** with proper orchestration

### The Orchestration Gap — Post-Juspay Chaos

The Indian payments ecosystem experienced a seismic shift in 2024-2025 when major payment aggregators severed ties with Juspay:

| Platform | Action |
|----------|--------|
| PhonePe | Ended Juspay partnership (Dec 2024) |
| Razorpay | Cut ties; launched "Optimizer" orchestration |
| Cashfree | Cut ties; launched "FlowWise" orchestration |
| Paytm | Ended integration (April 2025) |
| Juspay | Open-sourced Hyperswitch; pursuing merchant-direct model |

**The result:** Merchants who relied on Juspay as a single orchestration layer are now forced to directly integrate with each payment aggregator separately — a massive technical and compliance burden.

This created an orchestration vacuum. Juspay responded by open-sourcing **Hyperswitch** (Apache 2.0, PCI-certified, built in Rust) — now at **40,000 GitHub stars** and 4,600+ forks. Juspay CEO Vimal Kumar: "Open source for us is where people can contribute and use it themselves."

But Hyperswitch solves only payment routing. The deeper gap — **verify → decide → execute → allocate** — remains completely unaddressed. Nobody is doing for the full financial workflow what Hyperswitch did for payment orchestration.

### The 5 Gaps Nobody Has Filled

| Gap | What Happens Today | What Should Happen |
|-----|-------------------|-------------------|
| **Verify → Act** | KYC vendor says "verified." Human manually initiates next step. 3+ days. | Verification triggers rule → rule fires action → money moves. 4 seconds. |
| **Rule → Execute** | BRE decides. Output goes to Excel. Ops team manually processes. | Rule engine directly triggers Payout/PG/Verification via API. Zero human. |
| **Allocate → Disburse** | Finance team calculates splits in spreadsheet. Manual payout initiation. | Allocation rules auto-calculate co-lending splits, TDS, platform fees. Auto-disburse. |
| **Comply → Audit** | Decisions logged in 4 different systems. No single audit trail. | Every rule execution immutably logged with inputs, logic, outputs. Regulator-ready. |
| **Cloud → On-Prem** | 3-6 month enterprise implementation. Crores in licensing. | Docker pull. Config YAML. Live in hours. |

### Regulatory Pressure Is Real — 62 NBFCs Penalized

Between 2023–2025, RBI penalized **62 NBFCs** for compliance failures — including failure to implement suspicious transaction monitoring, failure to maintain updated KYC profiles, and deficiencies in vendor agreements.

This isn't theoretical risk. It's happening now. And the tools these NBFCs use — Lentra, Perfios, HyperVerge — don't close the compliance loop end-to-end.

**BRE adoption is already proving ROI:**
- Oxyzo (fintech lender): **30% reduction** in manual underwriting time, **40% improvement** in operational efficiency after BRE adoption
- PwC data: Lenders using BREs for lead filtering cut **customer acquisition costs by ~30%**
- India's fintech adoption rate: **87%** (vs. 67% global average)

### The On-Prem Gap Nobody Has Filled

The research confirmed: **no vendor currently positions a fully containerized, RBI-compliant, on-prem KYC/BRE stack** in public marketing. This is either a gap in the market or it happens only in private sales cycles.

Meanwhile:
- IDfy: Capterra reviewer confirmed "cannot be integrated with on-premise platforms"
- HyperVerge: On-prem not publicly confirmed
- Signzy: On-prem not publicly confirmed
- Lentra: Cloud-only
- Only Perfios confirms on-prem (microservices + containerized) — but they lack the execution layer

**Clawpay would be the first to combine: dockerized on-prem + BRE + financial execution + verification — in one stack.**

### RBI Co-Lending Directions 2025 — A Technology Forcing Function

Effective January 1, 2026, the new RBI CLA Directions require:

| Requirement | Impact on Technology |
|-------------|---------------------|
| **Mandatory borrower-level escrow** | Every co-lending pair needs automated escrow setup + maker-checker |
| **15-day settlement deadline** | Real-time allocation engine required — miss it and the full loan stays with originator |
| **NPA synchronization** | Near-real-time classification communication between institutions |
| **Blended rate + KFS auto-generation** | API-level integration between co-lenders |
| **NBFC minimum retention drops from 20% → 10%** | More complex allocation math, more partners per loan |

Most co-lending stacks are not built for this. The allocation engine in Clawpay — co-lending splits, conditional disbursement, time-based release, reconciliation — directly solves every one of these requirements.

Additionally, digital lending violations now carry penalties up to **₹250 crore under the DPDP Act**. RBI has already enforced: LenDenClub fined ₹1.99 Cr, LiquiLoans fined ₹1.92 Cr (August 2023). The compliance cost of getting this wrong is existential for mid-market NBFCs.

### RBI V-CIP Mandates Creating Forced Demand

RBI's V-CIP (Video KYC) framework creates non-negotiable requirements that most vendors struggle to meet:

- Technology infrastructure **must be housed on the Regulated Entity's premises**
- Third-party video platforms are **not permitted** for core V-CIP
- All data including video recordings **must be stored on servers located in India**
- V-CIP must be operated only by **specially trained officials of the RE**
- Mandatory **VAPT and Security Audits** (periodic, by CERT-In accredited agencies)

**For banks, on-prem is not a feature request — it's a regulatory mandate.** And most KYC vendors treat it as an afterthought.

---

## 3. What Is Clawpay?

### The One-Liner

**Clawpay is the open-core financial operations engine — compose, allocate, and automate money movement workflows, fully dockerized, live in hours.**

### The Philosophy (Inherited from OpenClaw)

OpenClaw's manifesto was: *"Your assistant. Your data. Your rules."*

Clawpay's manifesto: **"Your rules. Your money. Your infra."**

| OpenClaw Principle | Clawpay Translation |
|-------------------|---------------------|
| Local-first | Dockerized, on-prem, data never leaves your datacenter |
| Community skills | Community-built financial workflow templates (Cashfree-certified) |
| Any messaging channel | Any trigger — webhook, API, form submission, schedule, document upload |
| Autonomous execution | Rules fire → Cashfree actions execute, zero human touch |
| MIT licensed core | Open-core BRE engine on GitHub |
| Model agnostic | Rail agnostic — Cashfree default, others pluggable |

### The Architecture

```
┌──────────────────────────────────────────────────────────┐
│                  CLAWHUB MARKETPLACE                     │
│        Community templates · Cashfree certified          │
└────────────────────────┬─────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────┐
│                   CLAWPAY GATEWAY                        │
│              (OpenClaw-inspired core)                    │
│                                                          │
│  TRIGGER LAYER      RULE ENGINE       ACTION LAYER       │
│  ─────────────      ───────────       ────────────       │
│  Webhooks           Eligibility       Payment Gateway    │
│  Schedules          Allocation        Payouts            │
│  Form submits       Risk scoring      Verification       │
│  API events         Compliance        SmartOCR           │
│  Doc uploads        Workflow          FaceMatch          │
│                     Retry/Fallback    Liveness           │
│                                       Payment Links      │
│                                       Payment Forms      │
└────────────────────────┬─────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────┐
│            SIMULATION ENGINE                             │
│   Mock data → Rule preview → A/B test → Audit trail     │
└────────────────────────┬─────────────────────────────────┘
                         │
┌────────────────────────▼─────────────────────────────────┐
│           DOCKER / ON-PREM / CLOUD                       │
│         MIT licensed core · Cashfree powered             │
└──────────────────────────────────────────────────────────┘
```

### The 6 Rule Types — Core IP

**1. Eligibility Rules** — "Is this entity allowed to proceed?"
```
IF PAN verified = true
AND Aadhaar OCR confidence > 92%
AND FaceMatch score > 85%
AND CIBIL score > 680
THEN → eligible: true → trigger loan disbursal flow
ELSE → eligible: false → trigger manual review escalation
```

**2. Allocation Rules** — "How much goes where, and when?"
```
ON loan_disbursement_approved:
  Principal         → Borrower account (Bank Verify confirmed)
  Processing fee 2% → Platform wallet
  Co-lending split  → 80% Bank A / 20% NBFC B
  Insurance premium → Deduct ₹450 → Insurance partner
  Net disbursal     → Execute Cashfree Payout
```

**3. Risk Rules** — "What's the risk posture of this transaction?"

**4. Compliance Rules** — "Is this legal and within policy?" (AML checks, RBI limits, SAR logging)

**5. Workflow Rules** — "What's the next step in the journey?" (DAG-based, conditional branching)

**6. Retry & Fallback Rules** — "What happens when things fail?" (Auto-retry, UPI fallback, auto-refund)

### The Simulation Engine — The Demo That Sells Itself

Every competitor requires a sandbox account, API keys, test data setup, and a 45-minute onboarding call.

**Clawpay's pitch:**

> "Pull the Docker image. Open the browser. Build a loan disbursal flow. Hit simulate. Watch mock applicant data flow through every rule, every action, every edge case — in real time. No API keys. No account. No call."

```
MOCK APPLICANT: Rajesh Kumar
─────────────────────────
→ Aadhaar OCR: ✅ extracted (confidence: 94%)
→ PAN OCR: ✅ extracted (confidence: 98%)
→ FaceMatch: ✅ 91% match
→ Liveness: ✅ passed
→ Bank Verify: ✅ account active
→ CIBIL: ✅ score 720 (rule threshold: >680)
→ Eligibility Rule: ✅ APPROVED
→ Allocation:
    Loan amount: ₹2,00,000
    Processing fee (2%): ₹4,000 → Platform
    Insurance: ₹450 → Insurer
    Net disbursal: ₹1,95,550 → Rajesh's account
→ Payout: ✅ initiated (Cashfree Payouts)
→ Notification: ✅ SMS + webhook fired

Total flow time: 4.2 seconds
```

Any CTO can show this to their board and say "we can go live in a week."

---

## 4. Core Capabilities

### Capability 1: Orchestration Engine
Chain SmartOCR → FaceMatch → Liveness → Payment Form → Payout in a visual DAG-based flow. No code. Your ops team configures it.

### Capability 2: Allocation Engine (The Secret Weapon)
This is what every finance team maintains manually in Excel today:
- **Split rules** — 80% to vendor, 15% to platform, 5% to escrow
- **Conditional disbursement** — pay only if liveness passes + bank verified
- **Time-based release** — hold funds 7 days, then auto-release
- **Failure routing** — payout fails → retry → UPI fallback → alert
- **Reconciliation** — every allocation has an immutable audit trail

### Capability 3: Business Rule Engine (Next-Gen)
Not a linear workflow builder. A full DAG-based rule graph with:
- IF/THEN/ELSE/ELSE IF conditional branching
- No-code visual builder for ops teams
- Every decision logged + explained (regulator-ready)
- Maker-checker on every rule change (4-eyes mandatory)

### Capability 4: Docker-First On-Prem
```bash
docker pull clawpay/gateway
docker-compose up
# Live in 4 hours, inside your data center
```
RBI V-CIP compliant by architecture, not by paperwork. Zero data egress for Video KYC flows.

### Capability 5: Cashfree-Native Action Layer
Pre-wired to Cashfree primitives — the deepest, fastest integration:

| Cashfree Product | Clawpay Action |
|-----------------|----------------|
| SmartOCR | Extract document data |
| FaceMatch | Match faces against ID photos |
| Liveness | Detect real person vs. spoof |
| Payment Links | Collect money |
| Payment Forms | Structured payment UX |
| Payouts | Disburse funds |
| Bank Verify | Validate bank accounts |
| Payment Gateway | Accept payments |
| Verification Suite | Full KYC/KYB |

### Capability 6: Simulation & Audit
- Full mock preview with synthetic data — test before you go live
- A/B test rule configurations (champion vs. challenger)
- Immutable audit trail — every rule version, every execution, cryptographically signed
- RBI/compliance-ready reports generated out of the box

---

## 5. Use Cases

### Use Case 1: Loan Disbursement (NBFC / Co-Lending)

**The flow:**
```
Aadhaar OCR → PAN OCR → FaceMatch → Liveness → Bank Verify
→ Credit decision (eligibility rules)
→ Allocation: Co-lending split 80/20 + processing fee + insurance deduction
→ Net disbursal via Cashfree Payout → Borrower's account
→ Repayment collection via Payment Form
```

**Today:** 4–6 vendor integrations. One loan onboarding = weeks of engineering. Allocation done in Excel.

**With Clawpay:** 2-hour configuration. Entire flow runs autonomously. Every decision auditable.

**ICP:** Mid-market NBFCs (₹100–500 Cr AUM) scaling fast, under RBI compliance pressure, can't afford Lentra's pricing or timeline. 2,000+ of them in India.

---

### Use Case 2: Video KYC + Auto-Onboarding (Banks)

**The flow:**
```
Customer initiates Video KYC
→ Liveness check (Cashfree)
→ FaceMatch against Aadhaar photo
→ SmartOCR extracts document data
→ BRE evaluates eligibility rules in real time
→ IF score > threshold → auto-approve → trigger next action
   IF score borderline → route to human review queue
   IF score < threshold → auto-reject → notify with reason
→ Cashfree action fires (disburse / collect / activate)
→ Audit trail written, regulator-ready
```

**Today:** Agent only handles the video call. Everything after is manual — decision, logging, next step initiation.

**With Clawpay:** Agent handles borderline cases only. Clear cases auto-approved. All processing happens on-prem inside the bank's data center. RBI V-CIP compliant by architecture.

---

### Use Case 3: Vendor/Partner Payouts (B2B Marketplaces)

**The flow:**
```
Vendor onboarding → GST OCR → PAN verify → Bank Verify
→ Buyer payment via Payment Form
→ Allocation: Platform commission + GST TDS deduction + vendor payout
→ Auto-disburse via Cashfree Payouts
→ Reconciliation report generated
```

**Today:** 3 separate integrations. Finance team uses Excel for allocation. Payout initiated manually. Reconciliation takes days.

**With Clawpay:** One system. Rules calculate splits automatically. Payouts fire on rule trigger. Audit trail built-in.

---

### Use Case 4: Insurance Claims Processing

**The flow:**
```
Policyholder uploads medical docs → SmartOCR extraction
→ FaceMatch + Liveness (verify claimant)
→ Payment Form (premium verification)
→ Claim trigger → Allocation rules:
    Hospital share: ₹X
    Patient reimbursement: ₹Y
    Deductible applied: ₹Z
→ Payouts to hospital + patient
→ Audit log for IRDAI compliance
```

**Today:** Manual document review (60–70% ops cost). Separate systems for verification, decision, payment.

**With Clawpay:** SmartOCR alone saves 60–70% ops cost. Wrapped in a workflow with payment on either end.

---

### Use Case 5: Gig Worker Onboarding + Payouts

**The flow:**
```
Worker onboarding → SmartOCR (driving license/RC)
→ FaceMatch → Bank Verify → Activate
→ Payout on task completion
→ Allocation: Platform fee + insurance deduction + incentive + tax withholding
→ Earnings disbursed via Cashfree Payouts
```

**Today:** Onboarding a delivery partner takes 3–5 days due to manual KYC. Earnings splits done in code/Excel.

**With Clawpay:** Same-day onboarding. Automated earnings allocation. Every payout rule-governed and auditable.

---

## 6. Target ICPs

### Tier 1 — Immediate (Highest pain, clearest ROI)

| ICP | Rule Complexity | Allocation Complexity | Volume | Why They Buy |
|-----|----------------|----------------------|--------|-------------|
| **Mid-market NBFCs** (₹100–500 Cr AUM) | Very High | Very High | High | Co-lending splits, RBI compliance pressure, can't afford Lentra. 2,000+ in India. |
| **Co-lending platforms** | Very High | Very High | High | 80/20 splits between bank + NBFC. Allocation is literally their business. |
| **Digital lending startups** | High | High | Very High | Speed to market. Can't spend 6 months on BRE integration. |

### Tier 2 — Fast Follower

| ICP | Rule Complexity | Allocation Complexity | Volume | Why They Buy |
|-----|----------------|----------------------|--------|-------------|
| **Gig/Creator platforms** | Medium | Very High | Very High | Earnings splits, tax withholding, worker onboarding at scale |
| **B2B Marketplaces** | High | Very High | Medium | Escrow, vendor payouts, GST TDS, reconciliation |
| **Insurance / TPAs** | High | High | Medium | Claims processing, document verification, hospital payouts |

### Tier 3 — Platform Play

| ICP | Rule Complexity | Allocation Complexity | Volume | Why They Buy |
|-----|----------------|----------------------|--------|-------------|
| **Neobanks** | Very High | High | Very High | Full KYC → wallet activation → transact |
| **EdTech platforms** | Medium | Medium | High | Trainer revenue share, referral commissions, GST splits |
| **Healthcare aggregators** | High | High | Medium | Provider verification, patient payments, insurance routing |

---

## 7. Competitive Positioning

### The Landscape

```
                    EXECUTES AFTER VERIFY
                           │
                    HIGH   │   CLAWPAY ★
                           │   (verify + execute
                           │    + allocate + on-prem)
                           │
IDENTITY    ───────────────┼─────────────────── FINANCIAL
NATIVE         HyperVerge ●│                    NATIVE
               IDfy ●      │
               Signzy ●    │   Lentra ●
                           │   Perfios ●
                    LOW    │   (execute or decide,
                           │    not both)
```

**Clawpay owns a quadrant nobody else occupies** — identity-native AND financial-native AND execution-capable AND on-prem-first.

### Head-to-Head Comparison

| Dimension | Clawpay | HyperVerge | Lentra | Perfios | Temporal |
|-----------|---------|-----------|--------|---------|----------|
| **Core DNA** | Financial ops orchestration | Identity verification | Lending platform | Data aggregation + rules | Generic workflow engine |
| **Verify** | ✅ via Cashfree primitives | ✅ World-class (1B+ identities) | ❌ | ⚠️ Data-focused | ❌ |
| **Decide (BRE)** | ✅ Full DAG-based BRE | ⚠️ Linear workflow builder | ✅ Lending-specific | ⚠️ Bolted-on rules | ❌ (code-only) |
| **Execute** | ✅ Autonomous (PG + Payouts) | ❌ Stops at verification | ❌ Stops at decision | ❌ Stops at decision | ⚠️ Generic (no financial primitives) |
| **Allocate** | ✅ Co-lending, escrow, splits | ❌ | ❌ | ❌ | ❌ |
| **On-Prem** | ✅ Docker, live in hours | ⚠️ Available but painful | ⚠️ Enterprise only | ⚠️ Cloud-first | ✅ Self-hostable |
| **Setup Time** | Hours | Days–Weeks | 3–6 months | Months | Weeks (needs engineering) |
| **Pricing** | Usage-based, self-serve | Enterprise, quote-based | Crores in licensing | Enterprise pricing | $25/M actions + plan fee |
| **Open Source** | ✅ Open-core (MIT) | ❌ Proprietary | ❌ Proprietary | ❌ Proprietary | ✅ Core is open source |
| **Simulation** | ✅ Full mock preview | ❌ | ❌ | ❌ | ❌ |
| **Developer Experience** | SDK-first, webhook-native | API + SDK | Enterprise integration | Enterprise API | Code-first (Golang/Java/etc.) |

### Key Competitor Insights

**HyperVerge** — Revenue ₹149 Cr (FY25). 350+ clients (SBI, Bajaj, ICICI Securities, Grab). 30M+ onboardings/month, 99.5% accuracy, 200+ APIs. G2 reviewer: "Documentation can be improved to highlight key things like how to simulate different switch cases of the SDK." No confirmed on-prem. No financial execution or allocation capability.

**Perfios** — Unicorn ($1B+ valuation, March 2024). $263M total funding. Revenue undisclosed. IPO planned at ~$2B. 900+ FI clients, 1.7B transactions/year. InteGREAT platform is strong on data analytics but: "limited to financial analysis — does not offer end-to-end journeys or onboarding support." Complex setup. No execution layer.

**Lentra** — Revenue ₹217 Cr (FY25). $104M total funding. 50+ bank clients (HDFC, Federal, Standard Chartered). Full LOS + BRE stack (BREx). But: top-5 clients = 60% of revenue (concentration risk). Cloud-only. Implementation described as "time-consuming and costly."

**Signzy** — Revenue ₹111 Cr (FY24). $38.7M funding. 600+ FI clients (SBI, ICICI, HDFC, Axis). 340+ API marketplace, no-code GO platform. But: TrustRadius reviewer: "I will not recommend Signzy — they have very poor customer service." API downtime at peak hours. High pricing for mid-market.

**IDfy** — Revenue ₹188.5 Cr (FY25). $62.7M funding. 60M verifications/month. But Capterra reviewer confirmed: "Cannot be integrated with on-premise platforms." Pricing "not transparent" and "costly for small organizations."

**Temporal.io** — 2,500+ customers. $25/M actions. 450K+ actions/second. But: code-first only (no visual builder), steep learning curve, requires dedicated DevOps. Zero financial domain knowledge.

### Revenue & Funding Landscape (The Players You're Up Against)

| Company | Revenue (Latest) | Total Funding | Valuation | Monthly Volume | Key Gap |
|---------|-----------------|--------------|-----------|----------------|---------|
| HyperVerge | ₹149 Cr (FY25) | ~$2M disclosed | Undisclosed | 30M onboardings | No execution, no on-prem confirmed |
| Lentra | ₹217 Cr (FY25) | $104M | Undisclosed | 2M loans/month | Cloud-only, top-5 client concentration |
| Perfios | Undisclosed | $263M | $1B+ (unicorn) | 1.7B txns/year | Data-first, no end-to-end journeys |
| Signzy | ₹111 Cr (FY24) | $38.7M | ~$150M (2022) | 10M onboardings | Poor support, API downtime |
| IDfy | ₹188.5 Cr (FY25) | $62.7M | Undisclosed | 60M verifications | No on-prem (confirmed), opaque pricing |
| Cashfree Secure ID | Within Cashfree | $53M (2024 round) | Undisclosed | 20M daily API requests | No BRE, no allocation engine |

### The Delta 4 Framework (vs. HyperVerge specifically)

| Delta | Clawpay | HyperVerge | Magnitude |
|-------|---------|-----------|-----------|
| **Δ1: Execute** | Verify → Act autonomously | Verify → stop | 4x ops efficiency |
| **Δ2: BRE** | Open, explainable, configurable rules | Linear workflow builder | 4x rule power |
| **Δ3: On-Prem** | Docker, live in hours | 3–6 month implementation | 4x deployment speed |
| **Δ4: Allocate** | Co-lending, escrow, splits, TDS | Doesn't exist | ∞ — new capability |

---

## 8. Positioning & Messaging

### Positioning Statement

**For** mid-market NBFCs, lending platforms, and regulated financial institutions
**Who** need to automate verify → decide → execute → allocate workflows
**Clawpay is** the open-core financial operations engine
**That** composes Cashfree payment primitives into autonomous, rule-governed workflows — dockerized, simulation-ready, live in hours
**Unlike** HyperVerge (stops at verification), Lentra (stops at decision), and Temporal (has no financial domain knowledge)
**Clawpay** decides AND executes — and it's composable, self-hostable, and auditable by default.

### Core Messaging

**Tagline:**
> "Your rules. Your money. Your infra."

**One-liner:**
> "Clawpay is the workflow OS for financial operations — compose, allocate, and automate money movement with Cashfree primitives, fully dockerized, live in hours."

**The OpenClaw bridge:**
> "OpenClaw gave everyone a personal AI agent. Clawpay gives every fintech their financial operations agent — same philosophy, built for money movement."

**The competitor kill line:**
> "Lentra and Perfios decide. Then stop. Your ops team manually does the rest. Clawpay decides AND executes. Autonomously."

### Messaging by Persona

| Persona | What They Care About | Message |
|---------|---------------------|---------|
| **CTO / Engineering Head** | Speed to production, developer experience, no vendor lock-in | "Docker pull. Config YAML. Simulate. Go live. Open-core, no lock-in." |
| **Head of Risk / Compliance** | Auditability, explainability, regulatory alignment | "Every rule execution logged with inputs, logic, outputs. Regulator-ready reports out of the box." |
| **Head of Operations** | Reducing manual work, fewer systems, faster processing | "Replace 4 vendor integrations + Excel allocation with one configurable system." |
| **CFO / Business Head** | ROI, cost reduction, time-to-value | "Hours to deploy, not months. Usage-based pricing, not crores in licensing." |
| **CISO** | Data residency, on-prem, security posture | "Runs inside your data center. Zero data egress. Maker-checker on every rule change." |

### The 3 Proof Points

1. **Speed:** Live in hours, not months. Docker pull → configure → simulate → deploy.
2. **Completeness:** Verify → Decide → Execute → Allocate — the only system that closes the entire loop.
3. **Ownership:** Open-core. Self-hostable. You own your rules, your data, your infra. No vendor lock-in.

---

## 9. Go-To-Market Strategy

### Phase 1: Land with Lending (Q2 2026)
- **Target:** 1 design partner NBFC (mid-market, ₹100–500 Cr AUM)
- **Template:** Loan Disbursal Flow (end-to-end)
- **Goal:** Validate product, generate first case study
- **Motion:** Direct outreach via WTFraud community, Cashfree's existing NBFC relationships

### Phase 2: Expand to Gig + Marketplace (Q3 2026)
- **Target:** 2–3 horizontal customers
- **Templates:** Gig Worker Onboarding, Vendor Payout Flow
- **Goal:** Prove horizontal applicability

### Phase 3: Template Marketplace (Q4 2026)
- **Open:** ClawPay Hub — community-submitted, Cashfree-certified flow templates
- **Templates:** Loan Disbursal, Gig Onboarding, Vendor Payout, Insurance Claims — prebuilt flows
- **Goal:** Developer distribution + community moat

### Phase 4: Enterprise + On-Prem (2027)
- **Target:** Top 25 banks with V-CIP + BRE needs
- **Offer:** Docker-based on-prem deployment, CISO-ready documentation
- **Goal:** Enterprise revenue, long-term contracts

### Pricing Model

| Tier | Model | Who |
|------|-------|-----|
| **Open Core** | Free (MIT) | Developers, startups evaluating |
| **Cloud Managed** | Usage-based (per rule execution) | Growing NBFCs, fintechs |
| **Enterprise On-Prem** | Annual license + support | Banks, large NBFCs, insurance |

---

## 10. The Moat

1. **OpenClaw's Soul** — Open-core philosophy creates developer love and community distribution that proprietary vendors can never replicate
2. **Cashfree's Rails** — Native integration with PG, Payouts, and Verification Suite means the action layer works out of the box, no wiring needed
3. **Allocation Engine** — The unsexy, high-value capability that no competitor has. This is what every finance team does in Excel. Clawpay makes it a runtime config.
4. **Simulation Engine** — The demo that sells itself. No other BRE lets you test a complete flow with mock data before going live.
5. **On-Prem by Architecture** — Docker-first isn't an afterthought or enterprise upsell. It's the default deployment model. RBI compliant by design.
6. **Template Marketplace** — Community-contributed flows, Cashfree-certified, become a distribution flywheel that compounds over time.

---

## 11. What We're Asking From This Community

**To the 500+ product folks from NBFCs and banks in WTFraud:**

1. **Does this match a real pain you're dealing with?** Which of the 5 gaps (verify→act, rule→execute, allocate→disburse, comply→audit, cloud→on-prem) is the most painful for you right now?

2. **What flow would you want to automate first?** Loan disbursement? Video KYC onboarding? Vendor payouts? Something else?

3. **Would you pilot this?** If we built the loan disbursal template and the simulation engine — would you test it with mock data in your environment?

4. **What's your current stack?** How many vendors do you currently stitch together for a single onboarding flow? What's the setup time?

5. **What would make you say no?** Security concerns? Cashfree dependency? Pricing? Missing capability? Help us kill the bad ideas early.

---

*This document is a living draft. It will evolve based on community feedback, pilot learnings, and market response.*

*Built with the conviction that the next great fintech infrastructure company will be open-core, self-hostable, and composable — just like OpenClaw proved for personal AI agents.*

---

**Sources & References:**

- OpenClaw GitHub: 250K+ stars, MIT license, fastest-growing repo in GitHub history
- OpenClaw creator Peter Steinberger joined OpenAI (Feb 2026) — TechCrunch, CNBC
- ClawHavoc security incident: 1,184 malicious skills — The Hacker News, CyberPress, Antiy Labs
- KPMG India Fintech Report (Oct 2025): Lending + Payments = 60% of fintech funding
- BCG/QED Global Fintech Report (June 2025): India as global bright spot, $280B white space
- Global BRMS Market: $1.42B (2023) → $3.35B (2032) — SNS Insider, Grand View Research
- Decision Management Market: $6.92B (2025) → $19.34B (2032) — Fortune Business Insights
- RBI V-CIP Master Direction: On-prem mandate, data localization — RBI, IDfy, HyperVerge blogs
- Perfios: Unicorn at $1B+ (March 2024), IPO planned at ~$2B — Inc42, Business Standard
- HyperVerge: 1B+ identities verified, G2 reviews — G2, Tracxn
- Juspay orchestration collapse: PhonePe, Razorpay, Cashfree, Paytm exit — Inc42, Business Standard, The CapTable
- Temporal.io: $25/M actions pricing, 2,500+ customers — Temporal docs
- India digital lending: $133B revenue by 2030 — Inc42 H1 2025 report
