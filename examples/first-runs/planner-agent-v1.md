# Example: a personal agent for 6 senior planners

A full test conversation run with `prompt.md` (November 2026 prices, estimates).

---

**User:** I'd like a personal AI agent for each of our senior supply-chain planners: it reads their forecasts, ERP exports and notes, runs what-if scenarios and drafts plans.

---

**AI:** **Q1 — Volume.** How many requests per day, and roughly how long does a planner take per request today?
- A. Small: under ~100/day
- B. Medium: ~100 to 2,000/day
- C. Large: more than 2,000/day, or a big backlog

Or tell me in your own words.

---

**User:** B. About 50–100 requests a day per planner. I don't know how long a request takes a planner today.

---

**AI:** **Q2 — Users.** How many people will use it, and how?
- A. A few power users (≤ ~10, using it heavily all day)
- B. Many average users (dozens or more, a few requests a day each)
- C. Occasional users, or none (runs in the background)

Or tell me in your own words.

---

**User:** A. 6 senior planners, each working with it all day.

---

**AI:** **Q3 — Data sensitivity.** Can this data leave the company (sent to a cloud AI provider under a business contract)?
- A. Yes, ordinary business data
- B. Mixed: some sensitive, some not
- C. No, it must stay inside

Or tell me in your own words.

---

**User:** C. Demand forecasts and customer allocations are confidential: they must stay inside.

---

**AI:** I noted: hard, multi-step agent work (what-if scenarios, drafting plans) → difficulty C.

**Q5 — Duration.** How long will you need it?
- A. One-off / a project of a few weeks or months
- B. Permanent (part of the daily process)
- C. Not sure yet / pilot first

Or tell me in your own words.

---

**User:** B. Permanent.

---

**AI:** **Q6 — Checked examples.** Do you have examples where the correct answer is known (e.g. plans already validated)?
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more

Or tell me in your own words.

---

**User:** A. No checked examples.

---

**AI:** **Q7 — Internal skills.** Who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a workstation or a server
- C. We have an AI / ML or platform engineering team

Or tell me in your own words.

---

**User:** B. IT can install and maintain workstations; we have no ML team.

---

**AI:**

**Your case in one line:** personal planning agent (reads forecasts, ERP exports, notes; what-if scenarios; draft plans), ~300–600 requests/day, 6 power users, confidential data, hard multi-step, permanent.

**1. Recommended deployment: Open model on a workstation per user**
- Forecasts and customer allocations must stay inside (Q3 = C, R1) → no frontier cloud AI, no rented/hosted GPUs.
- 6 planners using it heavily all day (Q2 = A, R3) → one workstation each: no queue, cheapest per heavy user.
- Your IT can run workstations (Q7 = B, R8) → fine in-house; no small-model tuning or routing here, so no integrator is required.
- Hard, multi-step work (Q4 = C): a local open model handles agent work well but is below the best cloud models on the hardest cases (R6 cannot apply: data is confidential). The test in section 4 checks this.
- Concretely: 6 desk-side machines (e.g. Dell Pro Max with GB10, 128 GB), each running an open-weight model of ~100–200 billion parameters (compressed) as an agent ("agent" = a model that can use tools step by step) with read access to that planner's forecasts, ERP exports and notes.
- Capacity: ~a few hundred agent requests/day per machine vs 50–100 per planner → comfortable margin.

**2. Alternatives**
- **Shared on-site 8-GPU server** — main drawback: ≈ $220,000–450,000 plus ≥ 1 full-time person for only 6 users; built for hundreds.
- **Frontier cloud AI** — main drawback: data leaves the company (excluded by R1); also ≈ $1–10 per hard agent request → ≈ $6,300–130,000/month at 6,300–12,600 requests/month.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $53,000 | $120,000 |
| Running, per month (AI) | $750 | $1,600 |
| Human review of flagged items, per month | not known | not known |
| Total per year (AI only) | $8,900 | $19,000 |
| 3-year total (AI only) | $80,000 | $170,000 |
| Today's manual cost, per year (if known) | not known | |

Assumptions:
- Volume: 6 planners × 50–100 requests/day = 300–600/day; × 21 working days = 6,300–12,600 requests/month (your figures).
- Hardware: 6 workstations × $8,000–16,000 = $48,000–96,000 (reference figures).
- Integration (connect to ERP exports, forecast files and notes; test; train the 6 planners): $5,000–20,000, simple flow (reference). Setup = $53,000–116,000.
- Power: 6 × $20–50 = $120–300/month.
- IT upkeep: 0.05–0.1 FTE × $12,500 = $625–1,250/month. Running = $745–1,550/month.
- Per year = $745–1,550 × 12 = $8,940–18,600. 3-year = setup + 36 × monthly = $53,000 + $26,820 = $79,820 (low) and $116,000 + $55,800 = $171,800 (high).
- No cloud usage, no tuning (no checked examples, task not repetitive).
- Hardware not amortized in the monthly line (already in setup). For comparison only: ≈ $220–450/month per machine over 3 years, ≈ $1,300–2,700/month for 6.
- Human review: each planner checks the agent's drafts as part of the job; time per request today is not known, so this line and today's manual cost are not estimated.

**4. Before you decide: test on ~50 checked examples.** You have none yet, so have an expert check ~50 real planning requests first (about 1–2 days of work). Run them through the recommended option (one workstation) and one alternative (e.g. frontier cloud AI on anonymized copies of the same scenarios); compare accuracy, time per request and cost per request; check what goes wrong on the misses. Decide only then. *These are estimates; prices change fast and should be checked with suppliers.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes. One question per message, A/B/C plus free answer, questions in order, 6 questions asked (Q4 skipped with an "I noted" line because the description said "agent… what-if scenarios… drafts plans"), exact result format, closing offer. Rules shown: R1, R3, R8 (and R6 noted as blocked); R2, R4, R5, R7, R9, R10 correctly did not fire.
- **Arithmetic correct:** yes. 6 × $8,000 + $5,000 = $53,000; 6 × $16,000 + $20,000 = $116,000; $120 + $625 = $745; $300 + $1,250 = $1,550; × 12 = $8,940 / $18,600; 3-year = $79,820 / $171,800; rounded to 2 significant digits. Cloud comparison: 6,300 × $1 = $6,300, 12,600 × $10 = $126,000.
- **Recommendation coherent:** yes. Confidential data plus 6 heavy users points cleanly to a workstation per user; capacity (a few hundred requests/day per machine) covers 50–100 per planner; the quality gap on very hard tasks is flagged and left to the 50-example test.
- **Remaining prompt defects:** (1) the "Human review of flagged items" line has no rule when the time per item is unknown (only the manual-cost row says "not known"), and the "10–20% flagged" model does not fit an interactive personal assistant whose user reviews every answer; (2) "Total per year" does not say whether human review is included, so the AI had to label it "AI only"; (3) IT upkeep "0.05–0.1 FTE per 5–10 machines" is ambiguous for a given count (6 machines: 0.05–0.1 or pro-rated?); (4) section 4 says to compare with "one alternative" but does not say what to do when the alternative is excluded by R1 (here: anonymized copies); (5) minor: rounding of a halfway value (1,550 → 1,600) is not specified.
