# Test run: supplier e-mails (procurement director persona)

**User:** I want AI to help our buyers with supplier e-mails: extract key info (delivery date changes, price changes) and draft replies.

**AI:** Thanks, clear task. First question, **volume**: how many supplier e-mails per day, and how long does a buyer take per e-mail today?
- A. Small: under ~100/day
- B. Medium: ~100 to 2,000/day
- C. Large: more than 2,000/day
Or tell me in your own words.

**User:** C. About 400 buyers, each gets around 30 supplier e-mails a day, so roughly 12,000 a day. A buyer spends about 5 minutes per e-mail.

**AI:** Got it: ~12,000 e-mails/day, ~5 min each. **Users**: how will the buyers use it?
- A. A few power users using it all day
- B. Many average users, a few requests each
- C. Nobody directly, it runs in the background
Or tell me in your own words.

**User:** B. All 400 buyers, each a few times an hour. Average use, not all day.

**AI:** Noted. **Data sensitivity**: can these e-mails be sent to a cloud AI provider under a business contract?
- A. Yes, ordinary business data
- B. Mixed: some sensitive, some not
- C. No, it must stay inside the company
Or tell me in your own words.

**User:** C. They contain negotiated prices and supplier negotiations. They must stay inside.

**AI:** Understood. **Difficulty**: how hard is the work per e-mail?
- A. Repetitive (extract fields, fill a template)
- B. Medium (summarize, draft with some judgment)
- C. Hard, multi-step (reasoning across many documents, tools)
Or tell me in your own words.

**User:** B. Summarize and draft replies, with some judgment.

**AI:** Thanks. **Duration**: how long will you need it?
- A. One-off / a project of a few weeks or months
- B. Permanent, part of the daily process
- C. Not sure yet / pilot first
Or tell me in your own words.

**User:** B. Permanent.

**AI:** Noted. **Checked examples**: do you have e-mails where the correct extraction is already known (used to test models and tune a small one)?
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more
Or tell me in your own words.

**User:** B. A few dozen e-mails with checked extractions.

**AI:** Last one. **Internal skills**: who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a server
- C. We have an AI / ML or platform team
Or tell me in your own words.

**User:** B. IT can install and maintain servers. We have no ML team.

**AI:**

**Your case in one line:** extract key info from supplier e-mails and draft replies; ~12,000 e-mails/day; 400 average users; confidential (must stay inside); medium difficulty; permanent.

**1. Recommended deployment: Open model on a shared on-site server**
- Negotiated prices must stay inside (Q3 = C, R1) → no frontier cloud AI, no rented/hosted GPUs.
- 400 buyers, a few requests an hour each (Q2 = B, R4) → one shared server rather than one machine per person; expect some queueing (waiting) at peaks.
- Permanent (Q5 = B) → buying hardware makes sense; R2 (one-off) and R9 (pilot) do not apply.
- IT can run servers but there is no ML team (Q7 = B, R8) → plan 1 full-time person for the server and models (in-house or partly from a vendor).
- What it looks like: one 8-GPU server in your data center running an open-weight model (a model whose files you can download and run yourself), connected to the mail system so each supplier e-mail gets an extraction (new delivery date, new price) and a draft reply for the buyer to check.
- Capacity (rough): 12,000 e-mails/day is well inside "tens of thousands of documents per day", and 400 users is inside the ~200–1,000 range for one server.

**2. Alternatives**
- **Workstation per buyer** — main drawback: 400 machines ≈ $3.2–6.4 M to buy; does not scale to hundreds of people (R3 does not apply: these are not a few power users).
- **On-site routing** (small tuned model for the extraction, bigger open model for the drafts, all inside) — main drawback: more moving parts and integration cost; your ~50 checked extractions are just enough to try a tuned small model for the extraction part.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $250,000 | $550,000 |
| Running, per month | $18,000 | $24,000 |
| Running, per year | $220,000 | $290,000 |
| 3-year total | $910,000 | $1,400,000 |
| Today's manual cost (if known), per year | $16,000,000 | |

Assumptions:
- Volume: 400 buyers × 30 = 12,000 e-mails/day; ~21 working days/month → ~250,000 e-mails/month, ~3 M/year (my assumption on working days).
- Setup = server $220,000–450,000 + integration $30,000–100,000 (mail system, possibly ERP; reference figure for an on-site server with several systems). No tuning cost counted (task is medium, R5 does not fire); add ≈ $1,000–2,000 if you tune a small model for extraction.
- Monthly = server amortization over 5 years $3,700–7,500 + power/cooling/rack $2,000–4,000 + 1 FTE $12,500 = $18,200–24,000. No cloud usage.
- 3-year total = setup + 36 × monthly (as per method). Note: this counts the server both as a purchase and as monthly amortization; in pure cash terms (setup + 36 × power and staff) it is ≈ $770,000–1,100,000.
- Manual cost: 12,000 × 5 min = 1,000 hours/day × ~252 days = ~250,000 hours/year × $63/hour (reference rate, your salaries not given) ≈ $16 M/year.
- AI does **not** remove human review: buyers still check every draft before sending and handle flagged or uncertain e-mails, so real savings are a share of the $16 M, not all of it.
- Prices are November 2026 orders of magnitude; check them with suppliers.

**4. Before you decide: test on ~50 checked examples.** Run the same ~50 examples (with known correct answers) through the recommended option and one alternative; compare accuracy, time per item and cost per item; check what goes wrong on the misses. Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Protocol followed:** 7 questions, one per message, each with A/B/C options plus a free-answer line, about 5 lines each; no skips needed (the opening message did not clearly answer any question). Result uses the exact headings, table and closing offer. The user never needed "I don't know".
- **Arithmetic:** 400 × 30 = 12,000/day; 12,000 × 5 min = 1,000 h/day; monthly 3,700+2,000+12,500 = 18,200 and 7,500+4,000+12,500 = 24,000; annual 218,400 / 288,000 → $220k / $290k; 3-year 250,000 + 655,200 = 905,200 → $910k and 550,000 + 864,000 = 1,414,000 → $1.4M; manual 252,000 h × $63 ≈ $15.9M → $16M. All consistent with the reference table and capacity guide.
- **Recommendation coherent:** R1 (sensitive) + R4 (many average users) point unambiguously to the shared on-site server; alternatives are the right ones once cloud is excluded.
- **Defect 1 — no working-days convention:** the prompt gives per-day volumes but asks for monthly/annual costs without saying 21–22 working days vs 30 calendar days; manual cost changes by ~40% depending on the choice. The AI had to invent "21 days".
- **Defect 2 — 3-year total double-counts hardware:** "Setup = hardware purchase + …" and "Monthly = hardware amortization + …", then "3-year = setup + 36 × monthly" counts the server twice (≈ $130–270k over 3 years). The AI followed the formula but had to add a cash-view correction.
- **Defect 3 — gaps in rules for mixed/middle answers:** R8 says nothing for Q7 = B (does the IT team cover the ≥ 1 FTE or not?); R5 only fires for Q4 = A, though this task mixes repetitive extraction with medium drafting; R7/option 5 assume a cloud branch, so on-site-only routing for Q3 = C is not covered. Each forced a judgment call (whether to count tuning cost, whether to suggest routing).
