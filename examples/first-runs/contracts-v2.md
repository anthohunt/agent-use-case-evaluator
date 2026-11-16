# Example: 14,000 supplier contracts

A full test conversation run with `prompt.md` (November 2026 prices, estimates).

---

**User:** I want AI to review our 14,000 supplier contracts for risky clauses (penalties, auto-renewal, liability caps), and then the ~1,000 new contracts we sign every month.

---

**AI:** I noted: a backlog of 14,000 contracts, then ~1,000 a month (large volume, option C).

**Q1 — Time per contract.** How long does a person take to review one contract today?
- A. Under 1 hour
- B. 1 to 4 hours
- C. More than half a day

Or tell me in your own words.

---

**User:** B — a lawyer takes about 3 hours per contract, roughly $190.

---

**AI:** **Q2 — Users.** How many people will use it, and how?
- A. A few power users (≤ ~10 people using it heavily, all day)
- B. Many average users (dozens to thousands, a few requests a day each)
- C. Occasional users, or no users at all (it runs in the background as a batch)

Or tell me in your own words.

---

**User:** C — it runs as a batch. About 8 people in procurement legal would check the flagged outputs.

---

**AI:** **Q3 — Data sensitivity.** Can these contracts leave the company (sent to a cloud AI provider under a business contract)?
- A. Yes, it is ordinary business data
- B. Mixed: some contracts are sensitive, others are not
- C. No, it must stay inside (confidential prices, legal privilege…)

Or tell me in your own words.

---

**User:** B — about 70% contain negotiated prices or confidential terms that must not leave the company; about 30% are standard templates that could go to a cloud AI.

---

**AI:** **Q4 — Difficulty.** How hard is the review?
- A. Repetitive and standard (spot known clause types)
- B. Medium (summarize, compare with some judgment)
- C. Hard, multi-step (reasoning across many documents)

If it is a mix, say roughly what share is hard. Or tell me in your own words.

---

**User:** A, mostly — most clauses are standard, but about 20% of contracts are non-standard and need real judgment.

---

**AI:** **Q5 — Duration.** How long will you need it?
- A. One-off / a project of a few weeks or months
- B. Permanent (part of the daily process)
- C. Not sure yet / pilot first

Or tell me in your own words.

---

**User:** B — it's permanent.

---

**AI:** **Q6 — Checked examples.** Do you have contracts where the correct answer is known (already reviewed by a lawyer)? They are used to test models and to tune a small model.
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more

Or tell me in your own words.

---

**User:** C — about 300 contracts already reviewed by lawyers.

---

**AI:** **Q7 — Internal skills.** Who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a workstation or a server
- C. We have data-science, AI / ML or platform engineers (even a small team)

Or tell me in your own words.

---

**User:** C — IT can run servers, and there is a small data-science team.

---

**AI:**

**Your case in one line:** Flag risky clauses in supplier contracts — 14,000 backlog then ~1,000/month (≈ 48/working day), batch with 8 reviewers, mixed sensitivity (70% confidential), ~80% standard / 20% need judgment, permanent.

**1. Recommended deployment: Routing (mostly on site, small cloud branch)**
- Why:
  - Mixed data, 70% confidential (R1) → only the non-sensitive 30% may go to the cloud; routing keeps the rest inside.
  - Large backlog + mix of sensitive/non-sensitive and easy/hard (R7) → a triage model sends each contract to the right place.
  - Mostly standard + ~300 checked contracts (R5) → a small tuned model handles the standard 80% cheaply.
  - Batch of ~48 contracts/day (R10) → two workstations are enough; data-science team in house (R8) → no integrator needed.
- What it looks like:
  - Workstation 1: triage model + small tuned model (an open model adapted to your clauses) for the ~80% standard contracts.
  - Workstation 2: a bigger open model for non-standard contracts with confidential terms (~14% of contracts).
  - Non-standard standard-template contracts (~6%) → frontier cloud AI; uncertain cases → a lawyer.

**2. Alternatives**
- **Shared on-site 8-GPU server** — main drawback: ≈ $220,000–450,000 plus 1 full-time person, oversized for ~48 contracts a day.
- **Fully on-site routing (no cloud branch)** — main drawback: the ~6% hard non-sensitive contracts get below-frontier quality (but it saves a cloud contract that costs only ≈ $6–18/month).

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $47,000 | $130,000 |
| One-off backlog, if any (AI + review) | $130,000 | $130,000 |
| Running, per month (AI) | $300 | $370 |
| Human review of flagged items, per month | $9,500 | $9,500 |
| Total per year (AI + human review) | $120,000 | $120,000 |
| 3-year total | $530,000 | $620,000 |
| Today's manual cost, per year (if known) | $2.3 M | |

Assumptions:
- Flow 1,000 contracts/month; backlog 14,000 once (manual: ≈ $2.7 M and 42,000 hours, for comparison).
- Routes: 80% standard → small tuned model on site; 20% hard, of which 70% confidential → bigger open model on site (14%), 30% template → frontier cloud (6% = 60/month, 840 in the backlog) at ≈ $0.10–0.30 each.
- Setup: 2 workstations at $8,000–16,000 each + routing integration $30,000–100,000 + tuning $1,000–2,000; no vendor (in-house team).
- Running: cloud $6–18 + power $40–100 + IT upkeep 0.01 FTE per machine (2 × $125) per month; hardware not amortized here (≈ $220–450/month per workstation over 3 years, for comparison only).
- Review: lawyers check the 20% that need judgment (200/month, 2,800 in the backlog) in ≈ 1/4 of today's time (0.75 h, ≈ $47.50 each) → 150 h/month for 8 people; backlog ≈ 2,100 h (≈ 260 h each).
- Prices are November 2026 orders of magnitude; check with suppliers.

**4. Before you decide: test on ~50 checked examples.** Run the same ~50 examples (with known correct answers) through the recommended option and one feasible alternative (an option the rules did not exclude, or the same option with another model; if you have none yet, have an expert check ~50 real items first: about 1–2 days of work); compare accuracy, time per item and cost per item; check what goes wrong on the misses. Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes — one question per message, A/B/C options plus "Or tell me in your own words", 7 questions (Q1 asked only the missing time per contract, as the skip rule says), exact result format and closing offer.
- **Arithmetic correct:** yes, verified with Python (e.g. review 1,000 × 20% × $190/4 = $9,500/month; 3-year low 47,000 + 133,084 + 36 × 9,796 = $532,740 → $530,000; high 134,000 + 133,252 + 36 × 9,868 = $622,500 → $620,000; manual 12,000 × $190 = $2.28 M).
- **Recommendation coherent:** yes — routing on two workstations matches R1/R5/R7/R10; the quote shows that human review (≈ 97% of running cost) dominates and AI usage is negligible, which is the honest message (≈ $120,000/year vs ≈ $2.3 M today).
- **Remaining prompt defects:** (1) when Q1's volume is given but not the time per item, the prompt requires A/B/C options but provides none for time, so the AI had to invent them; (2) the review rule equates "flagged items" with "the share that needs judgment", so low = high and review rows show no range; (3) no staff time for maintaining the triage/tuned models (retuning, monitoring), only 0.01 FTE per workstation, so running cost is likely understated; (4) section 4 does not say to keep the ~50 test contracts out of the tuning set.
