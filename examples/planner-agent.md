# Example: a personal agent for 6 senior planners

A full test conversation run with `prompt.md` (November 2026 prices, estimates).

---

**User:** I'd like a personal AI agent for each of our senior supply-chain planners: it reads their forecasts, ERP exports and notes, runs what-if scenarios and drafts plans.

---

**AI:** **Q1 — Volume.** How many requests per day in total, and roughly how long does a planner take per request today?
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

**AI:** **Q6 — Checked examples.** Do you have examples where the correct answer is known (e.g. plans already validated by a senior planner)?
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
- C. We have data-science, AI / ML or platform engineers

Or tell me in your own words.

---

**User:** B. IT can install and maintain workstations; we have no ML team.

---

**AI:**

**Your case in one line:** personal planning agent (reads forecasts, ERP exports, notes; runs what-if scenarios; drafts plans), ~300–600 requests/day, 6 power users, confidential data, hard multi-step, permanent.

**1. Recommended deployment: Open model on a workstation per user**
- Forecasts and customer allocations must stay inside (Q3 = C, R1) → no frontier cloud AI, no rented/hosted GPUs.
- 6 planners using it heavily all day (Q2 = A, R3) → one workstation each: no queue, cheapest per heavy user.
- Your IT can run workstations (Q7 = B, R8) → fine in-house; no small-model tuning or routing here, so no integrator needed.
- Hard, multi-step work (Q4 = C): a local open model handles agent work well but is below the best cloud models on the hardest cases (R6 does not apply: data is confidential). The test in section 4 checks this.
- Concretely: 6 desk-side machines (e.g. Dell Pro Max with GB10, 128 GB), each running an open-weight model of ~100–200 billion parameters (compressed) as an agent ("agent" = a model that uses tools step by step), with access to that planner's forecasts, ERP exports and notes.
- Capacity: ~a few hundred agent requests/day per machine vs 50–100 per planner → comfortable margin.

**2. Alternatives**
- **Shared on-site 8-GPU server** — main drawback: ≈ $220,000–450,000 plus ≥ 1 full-time person (≈ $150,000/year) for only 6 users; built for hundreds.
- **Frontier cloud AI** — main drawback: data leaves the company (excluded by R1); also ≈ $1–10 per hard agent request → ≈ $6,300–130,000/month at 6,300–12,600 requests/month.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $53,000 | $120,000 |
| One-off backlog, if any (AI + review) | none | none |
| Running, per month (AI) | $870 | $1,100 |
| Human review of flagged items, per month | part of the user's work | part of the user's work |
| Total per year (AI + human review) | $10,000 | $13,000 |
| 3-year total | $84,000 | $150,000 |
| Today's manual cost, per year (if known) | not known | |

Assumptions:
- Volume: 6 planners × 50–100 requests/day = 300–600/day; × 21 working days = 6,300–12,600 requests/month (your figures).
- Hardware: 6 workstations × $8,000–16,000 = $48,000–96,000 (reference figures, ~3-year life).
- Integration (connect to ERP exports, forecast files and notes; test; train the 6 planners): $5,000–20,000, simple flow (reference). Setup = $53,000–116,000.
- Power: 6 × $20–50 = $120–300/month.
- IT upkeep: 6 × 0.01 FTE × $12,500 = $750/month. Running = $870–1,050/month.
- Per year = $870–1,050 × 12 = $10,440–12,600. 3-year = setup + 36 × monthly = $53,000 + $31,320 = $84,320 (low); $116,000 + $37,800 = $153,800 (high).
- No cloud usage, no tuning (no checked examples; task not repetitive).
- Hardware not in the monthly line (already in setup). For comparison only: ≈ $220–450/month per machine over 3 years, ≈ $1,300–2,700/month for 6.
- Human review: each planner checks the agent's drafts as part of their own work, so no separate cash line. Time per request today is not known, so today's manual cost is not estimated.

**4. Before you decide: test on ~50 checked examples.** You have none yet, so have an expert check ~50 real planning requests first (about 1–2 days of work). Run them through the recommended option (one workstation) and one feasible alternative (the same workstation with another open model); compare accuracy, time per request and cost per request; check what goes wrong on the misses. Decide only then. *These are estimates; prices change fast and should be checked with suppliers.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes. One question per message, A/B/C plus "Or tell me in your own words", questions in order, 6 questions asked (Q4 skipped with an "I noted" line, since the first message describes an agent running what-if scenarios and drafting plans), exact result format with every row, closing offer. Rules shown: R1, R3, R8 (R6 noted as blocked); R2, R4, R5, R7, R9, R10 correctly did not fire. Section 4 uses a feasible alternative (cloud is excluded by R1) and the "no examples yet → expert checks ~50" path.
- **Arithmetic correct:** yes (checked in Python). 6 × $8,000 + $5,000 = $53,000; 6 × $16,000 + $20,000 = $116,000; $120 + $750 = $870; $300 + $750 = $1,050; × 12 = $10,440 / $12,600; 3-year = $84,320 / $153,800; cloud comparison 6,300 × $1 = $6,300 and 12,600 × $10 = $126,000; all rounded to 2 significant digits.
- **Recommendation coherent:** yes. Confidential data plus 6 heavy users points cleanly to a workstation per user; per-machine capacity covers 50–100 requests/day; the quality gap on the hardest cases is flagged and left to the 50-example test.
- **Remaining prompt defects:** (1) two human-review rules overlap for a personal agent that drafts plans: "AI drafts something a person signs (plans) → read every item in ≈ 1/4 of today's time" vs "personal assistant → part of the user's work", with no precedence (the AI chose the second; with an unknown time per item both lead to no figure anyway); (2) the row label "Human review of flagged items" and "Total per year (AI + human review)" do not fit when review is "part of the user's work" (non-cash), so the total is AI-only without saying so; (3) rounding of a halfway value ($1,050 → $1,100 or $1,000) is not specified; (4) minor: on "I don't know", Q-level rule says "pick the most likely option", but for the time-per-item sub-question the quote rules say "not known": the two rules are consistent only if the reader connects them.
