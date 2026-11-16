# Example: 14,000 supplier contracts

A full test conversation run with `prompt.md` (November 2026 prices; all figures are estimates).

---

**User:** I want AI to review our 14,000 supplier contracts for risky clauses (penalties, auto-renewal, liability caps), and then the ~1,000 new contracts we sign every month.

---

**AI:** I noted: 14,000 contracts at once, then ~1,000/month.

**Q1 — Volume.** Please confirm the size, and tell me how long a person takes per contract today (and roughly what it costs).
- A. Small: under ~100 items/day
- B. Medium: ~100 to 2,000 items/day
- C. Large: over 2,000/day, or a big backlog then a steady flow

Or tell me in your own words.

---

**User:** C. A lawyer takes about 3 hours per contract today, roughly $190.

---

**AI:** **Q2 — Users.** How many people will use it, and how?
- A. A few power users (≤ ~10, using it all day)
- B. Many average users (dozens or more, a few requests a day)
- C. Occasional users, or none (runs in the background as a batch)

Or tell me in your own words.

---

**User:** C. It runs as a batch; about 8 people in procurement legal would check the flagged outputs.

---

**AI:** **Q3 — Data sensitivity.** Can these contracts be sent to a cloud AI provider under a business contract?
- A. Yes, ordinary business data
- B. Mixed: some are sensitive, others are not
- C. No, everything must stay inside

Or tell me in your own words.

---

**User:** B. About 70% contain negotiated prices or confidential terms that must not leave the company; about 30% are standard templates that could go to a cloud AI.

---

**AI:** **Q4 — Difficulty.** How hard is the review?
- A. Repetitive and standard (spot known clause types)
- B. Medium (summarize, compare with some judgment)
- C. Hard, multi-step (reasoning across many documents)

If it is a mix, roughly what share is hard?

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

**AI:** **Q6 — Checked examples.** Do you have contracts where the correct answer is known (already reviewed by a lawyer)? They are used to test models and tune a small model.
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more

Or tell me in your own words.

---

**User:** C. We have about 300 contracts already reviewed by lawyers.

---

**AI:** **Q7 — Internal skills.** Who would run the machines and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a workstation or server
- C. We have an AI / ML or platform engineering team

Or tell me in your own words.

---

**User:** C. IT can run servers, and we have a small data-science team.

---

**AI:** **Your case in one line:** risky-clause review of supplier contracts; 14,000 backlog then ~1,000/month (≈ 48/working day); batch, 8 reviewers; 70% confidential / 30% shareable; ~80% standard, ~20% need judgment; permanent.

**1. Recommended deployment: Routing (on 2 on-site workstations + frontier cloud AI for hard shareable contracts)**
- Mixed data, 70% must stay inside (Q3 = B, R1) and large backlog with a mix of easy and hard contracts (Q1 = C, R7) → routing, so only shareable contracts can go to the cloud.
- Mostly standard clauses + 300 lawyer-checked contracts (Q4 = A mostly, Q6 = C, R5) → a small tuned model (an open model trained on your examples) handles the standard part cheaply.
- Batch with no daily users, ~48 contracts/day after the backlog (Q2 = C, R10) → one or two workstations as batch machines are enough; no shared server needed.
- IT plus a data-science team (Q7 = C, R8) → realistic to build and run in-house.

What it looks like:
- Workstation 1: triage model (sorts each contract) + small tuned model for the ~80% standard contracts.
- Workstation 2: bigger open model for hard confidential contracts (~14% of contracts); hard shareable ones (~6%) go to frontier cloud AI.
- Uncertain or flagged clauses → the 8 procurement-legal reviewers.

**2. Alternatives**
- **Shared on-site 8-GPU server** — main drawback: ≈ $220,000–450,000 plus ≥ 1 full-time person, far too big for ~48 contracts/day.
- **Frontier cloud AI only** — main drawback: may only be used for the 30% standard templates; the 70% confidential contracts must stay inside (R1).

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $47,000 | $130,000 |
| One-off backlog, 14,000 contracts (AI + review) | $67,000 | $130,000 |
| Running, per month (AI) | $670 | $1,400 |
| Human review of flagged items, per month | $4,800 | $9,500 |
| Total per year | $65,000 | $130,000 |
| 3-year total | $310,000 | $660,000 |
| Today's manual cost, per year (if known) | $2,300,000 | |

Assumptions:
- Volumes: backlog 14,000 once; flow 1,000/month (your figures). Shares: 80% standard → small tuned model; 20% hard, of which 70% confidential (14% of all → on-site bigger model) and 30% shareable (6% of all → cloud), assuming sensitivity does not depend on difficulty.
- Setup: 2 workstations at $8,000–16,000 each ($16,000–32,000) + routing integration $30,000–100,000 + tuning $1,000–2,000 (+ ≈ $26 API). No vendor fees (in-house team).
- Running per month: cloud 60 contracts × $0.10–0.30 = $6–18; power 2 × $20–50 = $40–100; upkeep and model monitoring 0.05–0.1 FTE × $12,500 = $625–1,250. Hardware is in setup, not repeated here (spread over 3 years it would be ≈ $440–890/month).
- Human review: 10–20% of contracts flagged (default, you gave no figure), each checked in ≈ 1/4 of today's time (0.75 h × $63/h = $47.50). Flow: 100–200 contracts = 75–150 h/month (≈ 9–19 h per reviewer). Backlog: 1,400–2,800 contracts = 1,050–2,100 h ≈ 16–33 working days for your 8 reviewers at 8 h/day; backlog cloud cost only $84–252.
- Total per year = 12 × (AI + review). 3-year total = setup + backlog + 36 × (AI + review).
- Today: flow 12,000 contracts/year × $190 = $2.3 M/year (36,000 lawyer hours); the backlog alone would be ≈ $2.7 M (42,000 hours).
- Prices are November 2026 orders of magnitude; check them with suppliers.

**4. Before you decide: test on ~50 checked examples.** Run the same ~50 examples (with known correct answers) through the recommended option and one alternative (if you have none yet, have an expert check ~50 real items first: about 1–2 days of work); compare accuracy, time per item and cost per item; check what goes wrong on the misses. Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes. 7 questions, one per message, in order, each with A/B/C and a free-answer option; user's own figures used (3 h / $190, 8 reviewers, 70/30, 20%, 300 examples); result in the required headings, with the closing offer. Rules shown: R1, R7, R5, R10, R8.
- **Arithmetic correct:** yes, checked (unrounded: setup $47,000–134,000; backlog $66,584–133,252; monthly AI $671–1,368; review $4,750–9,500; per year $65,052–130,416; 3-year $308,740–658,500; manual $2,280,000/year).
- **Recommendation coherent:** yes. Routing on 2 workstations matches the capacity guide for a moderate flow; the cloud branch costs only ≈ $6–18/month, so it is kept for quality on hard shareable contracts, not for savings. Human review dominates running cost.
- **Remaining prompt defects:** (1) Q1 bundles volume and time per item but its options only cover volume, so it cannot be cleanly "skipped" when the description gives the volume. (2) The result table has no backlog row and the 3-year formula ignores the backlog, although "Backlog vs flow" asks to cost it separately; the AI had to add a row. (3) Q7 has no option for "IT + small data-science team" (between B and C). (4) IT upkeep is given per 5–10 machines only, and the 10–20% flag default ignores the user's own "20% need judgment" share; both needed a judgment call.
