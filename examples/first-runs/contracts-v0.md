# Test run — prompt.md, persona: supply-chain VP (contract review)

**User:** I want AI to review our 14,000 supplier contracts for risky clauses (penalties, auto-renewal, liability caps), and then the ~1,000 new contracts we sign every month.

**AI:** I noted: a 14,000-contract backlog, then ~1,000 contracts/month.
**Q1 — Volume.** Which fits best, and how long does a person take per contract today?
- A. Small: under ~100 items/day (≤ 2,000/month)
- B. Medium: ~100 to 2,000 items/day
- C. Large: more than 2,000/day, or a big backlog
Or tell me in your own words.

**User:** C. The 14,000 backlog first, then ~1,000 a month. A lawyer takes about 3 hours per contract today, roughly $190.

**AI:** **Q2 — Users.** How many people will use it, and how?
- A. A few power users (≤ ~10, using it heavily all day)
- B. Many average users (dozens to thousands, a few requests a day)
- C. Occasional users, or none (it runs in the background as a batch)
Or tell me in your own words.

**User:** C. It runs as a batch; about 8 people in procurement legal would check the flagged outputs.

**AI:** **Q3 — Data sensitivity.** Can these contracts leave the company (sent to a cloud AI provider under a business contract)?
- A. Yes, ordinary business data
- B. Mixed: some items are sensitive, others are not
- C. No, they must stay inside (confidential prices, legal privilege…)
Or tell me in your own words.

**User:** B. About 70% contain negotiated prices or confidential terms that must not leave; ~30% are standard templates that could go to a cloud AI.

**AI:** **Q4 — Difficulty.** How hard is the task?
- A. Repetitive and standard (extract clauses, classify, flag)
- B. Medium (summarize, compare with some judgment)
- C. Hard, multi-step (reasoning across many documents, tools, an "agent")
Or tell me in your own words.

**User:** A, mostly. Most clauses are standard, but about 20% of contracts are non-standard and need real judgment.

**AI:** **Q5 — Duration.** How long will you need it?
- A. One-off / a project of a few weeks or months
- B. Permanent (part of the daily process)
- C. Not sure yet / pilot first
Or tell me in your own words.

**User:** B. It's permanent.

**AI:** **Q6 — Checked examples.** Do you have contracts where the correct answer is known (already reviewed by a lawyer)? They are used to test models and tune a small model.
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more
Or tell me in your own words.

**User:** C. We have about 300 contracts already reviewed by lawyers.

**AI:** **Q7 — Internal skills.** Who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a workstation or a server
- C. We have an AI / ML or platform engineering team
Or tell me in your own words.

**User:** B. IT can run servers, and there is a small data-science team.

**AI:**

**Your case in one line:** Flag risky clauses in supplier contracts — 14,000 backlog then ~1,000/month; batch with ~8 legal reviewers; 70% confidential / 30% shareable; ~80% standard, ~20% need judgment; permanent.

**1. Recommended deployment: Routing (mostly on site, small cloud share)**
- Why:
  - Mixed data, 70% confidential (Q3 = B, R1) → confidential contracts never leave; only standard templates may go to the cloud.
  - Large backlog + mixed sensitivity and difficulty (Q1 = C, Q3 = B, R7) → a triage model sends each contract to the right place.
  - Mostly standard clauses + ~300 lawyer-checked contracts (Q4 = A, Q6 = C, R5) → a small tuned model (an open model trained a little on your examples) handles the easy ~80%.
  - Permanent need, IT can run machines (Q5 = B, Q7 = B) → buying a little hardware pays off; your data-science team can do the tuning.
- What it looks like:
  - 1–2 powerful workstations in your data center: one runs the triage + small tuned model on all contracts; a bigger open model (~100–200 billion parameters) handles the ~14% that are both non-standard and confidential.
  - The ~6% non-standard **and** non-confidential contracts go to a frontier cloud AI.
  - Uncertain or flagged contracts → your 8 procurement lawyers.

**2. Alternatives**
- **Shared on-site 8-GPU server** — main drawback: ≈ $220k–450k plus a full-time person, for only ~50 contracts per working day.
- **Frontier cloud AI for everything** — main drawback: 70% of contracts are confidential and cannot leave (R1).

**3. Estimated quote** (orders of magnitude, November 2026 prices — check with suppliers)

| | Low | High |
|---|---|---|
| Setup (one-time) | $39,000 | $130,000 |
| Running, per month | $1,500 | $4,100 |
| Running, per year | $18,000 | $50,000 |
| 3-year total | $93,000 | $280,000 |
| Today's manual cost (if known), per year | $2,300,000 | |

Assumptions:
- Volume: 1,000 contracts/month ongoing; backlog of 14,000 once (manual backlog ≈ $2.7 M and ≈ 42,000 lawyer hours; ongoing ≈ 3,000 hours/month). Your figures: 3 h and $190 per contract.
- Routes (assuming difficulty and sensitivity are independent): 80% easy → small tuned model on site; 14% hard + confidential → bigger open model on site; 6% hard + shareable → cloud (60 contracts/month).
- Hardware: low = 1 workstation at $8,000; high = 2 multi-GPU towers at $16,000 each; 3-year life → ≈ $220–890/month; power ≈ $20–100/month.
- Cloud: $0.10–0.30 per hard contract × 60/month ≈ $6–18/month; backlog share (840 contracts) ≈ $84–250 once (not significant).
- Integration (routing, document store, tests, training): $30,000–100,000. Tuning: $1,000–2,000 (≈ 1 day of engineer time). No vendor fees (your IT runs it).
- Staff: 0.1–0.25 of a full-time person for IT/data science ≈ $1,250–3,100/month.
- **Not included:** your lawyers' time on flagged contracts. AI does **not** remove human review — keep a person on flagged and uncertain items.

**4. Before you decide: test on ~50 checked examples.** Take ~50 of your 300 lawyer-reviewed contracts (mix standard and non-standard), run them through the routing setup and through one alternative (e.g. the on-site open model alone); compare accuracy, time per contract and cost per contract; look at what goes wrong on the misses. Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Protocol followed.** One question per message, 7 questions (the maximum allowed), A/B/C options plus a free-answer invitation, all within 4–6 lines; the result uses the exact headings and table, and ends with the required offer. Q1 was not skipped even though the volume was already given, because the time per contract was missing; the AI used "I noted:" and still asked. This is a reasonable reading but not one the prompt states.
- **Arithmetic checks out.** Setup 8k+30k+1k = 39k and 32k+100k+2k = 134k → $130k. Monthly 222+20+6+1,250 ≈ $1,500 and 889+100+18+3,125 ≈ $4,100. Yearly 17,976 → $18k and 49,584 → $50k. 3-year totals 92,928 → $93k and 282,752 → $280k. Manual cost 12,000 × $190 = $2.28M → $2.3M per year. The backlog figures ($2.7M, 42,000 h, $1,400 at $0.10) match the reference table.
- **Recommendation hangs together.** R1, R5 and R7 all point to routing. The AI chose workstations over the 8-GPU server because the capacity guide says a workstation handles a few thousand documents a day, but the prompt's routing description never says what on-site hardware routing should use.
- **Prompt defects that caused hesitation:**
  - (1) Q1 asks for two things (volume and time per item) and has no option for "big backlog + small steady flow". 1,000/month is option A, but the backlog makes it C.
  - (2) Q4 has no "mixed difficulty" option, even though R7 talks about mixed difficulty. R7's condition (Q1 = C, Q3 = B) does not test difficulty at all.
  - (3) No rule covers Q2 = C (batch) or Q7 = B. R8 only covers A and C, and "IT + small data-science team" falls between B and C.
  - (4) The quote formula leaves out the reviewers' time on flagged items, even though the prompt insists human review stays. So the "manual vs AI" comparison is lopsided.
  - (5) The quote format does not separate a one-off backlog from steady-state running cost.
  - (6) There is no guidance on how many workstations routing needs or on a cost per hard item; the AI picked 1–2 and $0.10–0.30 itself.
