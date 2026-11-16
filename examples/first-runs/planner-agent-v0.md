# Test run: senior supply-chain planners (prompt.md)

**User:** I'd like a personal AI agent for each of our senior supply-chain planners: it reads their forecasts, ERP exports and notes, runs what-if scenarios and drafts plans.

**AI:** I noted: a personal planning agent for each senior planner (forecasts, ERP exports, notes, what-if scenarios, draft plans).

**Q1 of 7: Volume.** How many requests per day or per month? And roughly how long does a planner take per request today?
- A. Small: under ~100/day (≤ 2,000/month)
- B. Medium: ~100 to 2,000/day
- C. Large: more than 2,000/day, or a big backlog

Or tell me in your own words.

**User:** B. Each planner would send about 50–100 requests a day, so 300–600 a day in total. How long a request takes today, I don't know: their time is valuable, but I can't put a number on it.

**AI:** **Q2 of 7: Users.** How many people will use it, and how?
- A. A few power users (≤ ~10, using it heavily all day)
- B. Many average users (dozens to thousands, a few requests a day each)
- C. Occasional users, or none (it runs in the background as a batch)

Or tell me in your own words.

**User:** A. 6 senior planners, each working with it all day.

**AI:** **Q3 of 7: Data sensitivity.** Can this data leave the company (sent to a cloud AI provider under a business contract)?
- A. Yes, ordinary business data
- B. Mixed: some items sensitive, others not
- C. No, it must stay inside

Or tell me in your own words.

**User:** C. Demand forecasts and customer allocations are confidential: they must stay inside.

**AI:** **Q4 of 7: Difficulty.** How hard is the task?
- A. Repetitive and standard (extract, classify, fill a template)
- B. Medium (summarize, draft, compare with some judgment)
- C. Hard, multi-step (reasoning across many documents, using tools, planning: an "agent")

Or tell me in your own words.

**User:** C. It's hard, multi-step work.

**AI:** **Q5 of 7: Duration.** How long will you need it?
- A. One-off / a project of a few weeks or months
- B. Permanent (part of the daily process)
- C. Not sure yet / pilot first

Or tell me in your own words.

**User:** B. Permanent.

**AI:** **Q6 of 7: Checked examples.** Do you have examples where the correct answer is known (e.g. past plans reviewed and approved)? They are used to test models.
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more

Or tell me in your own words.

**User:** A. No checked examples.

**AI:** **Q7 of 7: Internal skills.** Who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a workstation or a server
- C. We have an AI / ML or platform engineering team

Or tell me in your own words.

**User:** B. IT can install and maintain workstations. We have no ML team.

**AI:**

**Your case in one line:** personal planning agent for 6 senior supply-chain planners; ~300–600 requests/day; confidential data (must stay inside); hard, multi-step; permanent.

**1. Recommended deployment: Open model on a workstation per user**
- Forecasts and customer allocations must stay inside (Q3 = C, R1) → no frontier cloud AI, no rented or hosted GPUs.
- 6 power users working all day (Q2 = A) with an on-site requirement (R3) → one workstation per planner: no queue, cheapest per heavy user.
- Your IT team can install and maintain workstations (Q7 = B, R8) → no vendor setup contract needed; a shared server would need ≥ 1 full-time person you do not have.
- Hard, multi-step task (Q4 = C): R6 would point to frontier cloud AI, but R1 wins. So use the strongest open model a workstation can run, and test it on real planning cases first (section 4).
- Concretely: 6 desk-side machines (e.g. Dell Pro Max with GB10, 128 GB, or a multi-GPU tower), each running an open-weight model (an AI model you can download and run yourself) of roughly 100–200 billion parameters (compressed).
- Each machine reads that planner's forecasts, ERP exports and notes locally, runs the what-if scenarios with tools (spreadsheet or planning calculations), and drafts the plan for the planner to check.

**2. Alternatives**
- **Open model on a shared on-site server**: main drawback: ≈ $220,000–450,000 up front plus ≥ 1 full-time person (≈ $12,500/month) for only 6 users. It only pays off with hundreds of users.
- **Frontier cloud AI under a dedicated private hosting contract**: main drawback: data leaves the building (only possible if your policy accepts such a contract), and hard agent tasks cost ≈ $1–10 each. At ~6,300–13,000 requests/month that is ≈ $6,300–130,000/month.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $53,000 | $120,000 |
| Running, per month | $2,100 | $4,300 |
| Running, per year | $25,000 | $51,000 |
| 3-year total | $130,000 | $270,000 |
| Today's manual cost (if known), per year | not known (time per request not given) | |

Assumptions:
- 6 workstations at ≈ $8,000 (low) to ≈ $16,000 (high) each: $48,000–96,000.
- Integration (reading forecast files, ERP exports and notes; testing; training the planners): ≈ $5,000–20,000, the "simple flow" range, because each machine works on its own planner's files. If you need live connections to several systems, count $30,000–100,000 instead.
- No tuning: no checked examples, and the task is not repetitive. No vendor fees: your IT runs the machines.
- Amortization over 3 years: ≈ $220–450/month per machine, so $1,320–2,700/month for 6.
- Power ≈ $20–50/month per machine, so $120–300/month.
- IT time to maintain 6 workstations: my assumption is ≈ 0.05–0.1 FTE (a full-time employee) × $12,500 = $625–1,250/month. Please correct it.
- Cloud usage: $0 (everything runs locally).
- Monthly: $2,065 (low) to $4,250 (high), rounded. Per year: × 12. 3-year total: setup + 36 × monthly.
- Volume: 6 planners × 50–100 requests/day = 300–600/day, about 21 working days → ≈ 6,300–13,000/month. One heavy user per workstation fits the capacity guide.
- Manual cost: you did not give the time per request, so I cannot compute it. At ≈ $63/hour (reference figure), each hour the agent saves per planner per day is worth ≈ $63 × 21 × 12 ≈ $16,000/year per planner. AI does **not** remove human review: the planner stays responsible for every plan.
- Prices are November 2026 orders of magnitude. Check them with suppliers.

**4. Before you decide: test on ~50 checked examples.** Run the same ~50 examples (with known correct answers) through the recommended option and one alternative; compare accuracy, time per item and cost per item; check what goes wrong on the misses. Decide only then. You have no checked examples today, so first have your planners collect ~50 past cases with the plan they actually approved. You can start with one workstation as a test before buying six. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt compliance:** the AI asked one question per message, 7 questions in total (the maximum allowed), each with A/B/C plus a free answer, in the prescribed order, and kept each question to 4–6 short lines. It used the result format headings and table and ended with the required offer. It skipped no question: the opening description did not clearly answer any of them (Q4 was only implied).
- **Arithmetic is correct:** setup is 6 × $8k + $5k = $53k, and 6 × $16k + $20k = $116k, rounded to $120k. Monthly is 1,320 + 120 + 625 = $2,065 (low) and 2,700 + 300 + 1,250 = $4,250 (high). Per year is $24,780 → $25k and $51,000. The 3-year total is 53,000 + 74,340 = $127k → $130k and 116,000 + 153,000 = $269k → $270k. All of this matches the reference figures (≈ $220–450/month amortization, $20–50 power, $12,500/FTE-month, the $1–10 agent-task range for the cloud alternative). One small inconsistency: annual figures come from the unrounded monthly amounts, so $4,300 × 12 = $51,600 does not quite equal the $51,000 shown.
- **The recommendation is coherent:** R1 (exclude cloud and rented GPUs) and R3 (workstation per power user) fire, and the conflict order (R1 > R3/R4 > cost) is applied correctly. R6 is correctly overridden by R1, and the AI says so openly. R2, R4, R5, R7 and R9 do not apply. R8 is neutral for Q7 = B.
- **Defect 1, missing figure (staff time for workstations):** the quote formula includes "staff time (FTE fraction × $12,500)", but the reference table gives no FTE share for workstations (it gives one only for the server and rented GPUs). The AI had to invent 0.05–0.1 FTE. That breaks the "never invent figures" spirit and adds about 30% to the low monthly figure.
- **Defect 2, unknown time per item:** Q1 bundles two questions (volume + time per item), and the prompt has no fallback when the user does not know the time. "Assume ≈ $63/hour" covers the rate, not the duration, so the "Today's manual cost" row stays empty and the value case is weak. The integration range is also ambiguous: $30k–100k is tied to "routing or an on-site server", so a workstation setup with several systems (ERP, forecasts, notes) falls between the two brackets.
- **Defect 3, no guidance on quality and capacity for hard on-site tasks, and an unworkable test step:** the prompt gives no guidance on whether a 100–200B open model on a workstation handles hard multi-step agent work, or how many agent requests per day one workstation can sustain. The Q4 = C + Q3 = C combination is left to the AI's judgment. Section 4 then mandates a test on "~50 checked examples" even when Q6 = A, without saying how to get them. The prompt also does not say whether to round before or after multiplying (see the $51k vs $51.6k mismatch).
