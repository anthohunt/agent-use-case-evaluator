# Example: 14,000 supplier contracts

A full test conversation run with `prompt.md` (November 2026 prices, estimates).

---

**User:** I want AI to review our 14,000 supplier contracts for risky clauses (penalties, auto-renewal, liability caps), and then the ~1,000 new contracts we sign every month.

---

**AI:** I noted: a backlog of 14,000 contracts, then ~1,000 new ones per month (volume C: big backlog, then a steady flow).

How long does a person take per contract today, and roughly what does that cost?

---

**User:** About 3 hours per contract with a lawyer, roughly $190.

---

**AI:** **Q2 — Users.** How many people will use it, and how?
- A. A few power users (≤ ~10 people using it heavily, all day)
- B. Many average users (dozens to thousands, a few requests a day each)
- C. Occasional users, or no users at all (it runs in the background as a batch)

Or tell me in your own words.

---

**User:** C. It runs as a batch; about 8 people in procurement legal would check the flagged outputs.

---

**AI:** **Q3 — Data sensitivity.** Can these contracts leave the company (sent to a cloud AI provider under a business contract)?
- A. Yes, it is ordinary business data
- B. Mixed: some items are sensitive, others are not
- C. No, it must stay inside (confidential prices, personal data, legal privilege…)

Or tell me in your own words.

---

**User:** B. About 70% contain negotiated prices or confidential terms that must not leave the company; about 30% are standard templates that could go to a cloud AI.

---

**AI:** **Q4 — Difficulty.** How hard is the review?
- A. Repetitive and standard (spot known clause types, classify)
- B. Medium (summarize, compare with some judgment)
- C. Hard, multi-step (reasoning across many documents, tools, planning)

If it is a mix, say roughly what share is hard. Or tell me in your own words.

---

**User:** A, mostly. Most clauses are standard, but about 20% of contracts are non-standard and need real judgment.

---

**AI:** **Q5 — Duration.** How long will you need it?
- A. One-off / a project of a few weeks or months
- B. Permanent (part of the daily process)
- C. Not sure yet / pilot first

Or tell me in your own words.

---

**User:** B. It's permanent.

---

**AI:** **Q6 — Checked examples.** Do you have contracts where the correct answer is known (already reviewed by a lawyer)? They are used to test models and to tune a small model.
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more

Or tell me in your own words.

---

**User:** C. About 300 contracts already reviewed by lawyers.

---

**AI:** **Q7 — Internal skills.** Who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a workstation or a server
- C. We have data-science, AI / ML or platform engineers (even a small team)

Or tell me in your own words.

---

**User:** C. IT can run servers, and we have a small data-science team.

---

**AI:** **Your case in one line:** review supplier contracts for risky clauses; 14,000 backlog then ~1,000/month; batch, 8 legal reviewers check flags; mixed data (70% confidential, 30% standard templates); 80% standard, 20% need judgment; permanent.

**1. Recommended deployment: Routing (2 on-site workstations + frontier cloud AI for non-sensitive hard contracts)**
- Mixed data (Q3 = B, R1) and large backlog with mixed easy/hard items (Q1 = C, R7) → routing, so only the 30% standard-template contracts can ever go to the cloud.
- Mostly standard + ~300 lawyer-reviewed contracts (Q4 mix, Q6 = C, R5) → a small tuned open model handles the standard 80%.
- Batch, no interactive users (Q2 = C, R10) at ~48 contracts per working day → two workstations are enough; no shared server needed.
- IT + small data-science team (Q7 = C, R8) → setup, tuning and running can be done in-house.
- Concretely: workstation 1 runs a small triage model (sorts each contract) plus the small tuned model (standard contracts); workstation 2 runs a bigger open model for non-standard confidential contracts (~14% of items); non-standard template contracts (~6%) go to frontier cloud AI; uncertain cases go to the 8 procurement-legal reviewers.

**2. Alternatives**
- **Fully on-site routing (no cloud branch)** — main drawback: the ~6% hard template contracts get open-model quality instead of frontier quality (small saving, slightly lower accuracy).
- **Shared on-site 8-GPU server** — main drawback: ≈ $220,000–450,000 plus ≥ 1 full-time person for ~50 contracts a day; oversized.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $47,000 | $130,000 |
| One-off backlog, if any (AI + review) | $80,000 | $220,000 |
| Running, per month (AI) | $920 | $1,600 |
| Human review (flagged or all items), per month | $5,700 | $16,000 |
| Total per year (AI + human review) | $79,000 | $210,000 |
| 3-year total | $370,000 | $980,000 |
| Today's manual cost, per year (if known) | $2,300,000 | |

Assumptions:
- Volume: 14,000 backlog contracts once; 1,000 new contracts/month (≈ 48 per working day, 21 days/month).
- Routes: 80% standard → small tuned model on site; 20% non-standard, of which 70% confidential (14% of all) → bigger open model on site and 30% templates (6% of all) → frontier cloud AI at ≈ $0.10–0.30 per hard contract.
- Setup: 2 workstations at ≈ $8,000–16,000 each; integration for routing ≈ $30,000–100,000; tuning ≈ $1,000–2,000 engineer time + ≈ $26 API; no vendor fees (in-house team).
- Running: cloud ≈ $6–18; power ≈ $20–50 per workstation; IT upkeep 0.01 FTE per workstation (≈ $125 each); monitoring/retuning 0.05–0.1 FTE (≈ $625–1,250); FTE = $12,500/month. Hardware not amortized in the monthly line (it is in setup); for comparison ≈ $220–450/month per workstation over 3 years.
- Human review: lawyers check 15–25% flagged contracts (around your 20% non-standard) at 1/5–1/3 of today's 3 h (≈ $38–63 per contract); that is ≈ 150–250 contracts and ≈ 90–250 hours per month for 8 people; the backlog adds ≈ 2,100–3,500 contracts once.
- Today: $190 per contract (3 h at ≈ $63/hour); 12,000 contracts/year ≈ $2.3 M/year; the backlog done manually would be ≈ $2.7 M and ≈ 42,000 hours.

**4. Before you decide: test on ~50 checked examples.** Run the same ~50 lawyer-reviewed contracts (with known correct answers) through the recommended routing and one feasible alternative (fully on-site routing, or the same routing with another open model); keep these test contracts out of the ~250 used to tune the small model; compare accuracy, time per contract and cost per contract; check what goes wrong on the misses (especially missed penalty, auto-renewal and liability-cap clauses). Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes. One question per message; the volume part already given was noted and only the missing time per contract was asked as a short open question; 7 questions in total (Q1 partial + Q2–Q7); result in the exact format, followed by the offer to redo the quote.
- **Arithmetic correct:** verified in Python. Setup $47,026–134,026; backlog $79,884–221,919; AI running $921–1,618/month; review $5,700–15,833/month; per year $79,452–209,416; 3-year $365,266–984,193; today $2,280,000/year. Rounded to 2 significant digits.
- **Recommendation coherent:** yes. R1 (mixed) and R7 lead to routing, R5 adds the small tuned model, R10 sizes it at two workstations, R8 keeps it in-house; the cloud branch only sees template contracts.
- **Remaining prompt defects:** (1) the prompt does not say whether the open question for a partly answered Q1 counts toward the 5–7 questions (here it was counted, giving 7). (2) The routing branch for hard non-sensitive items has two possible unit prices ($0.10 per standard document vs $0.10–0.30 per hard document); this run used $0.10–0.30. (3) The result table has no row for today's manual cost of the backlog (≈ $2.7 M), which is the most striking comparison for a backlog case; it had to go into the assumptions.
