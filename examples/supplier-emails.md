# Example: supplier e-mails for 400 buyers

A full test conversation run with `prompt.md` (November 2026 prices; all figures are estimates).

---

**User:** I want AI to help our buyers with supplier e-mails: extract key info (delivery date changes, price changes) and draft replies.

---

**AI:** **Q1 — Volume.** How many supplier e-mails per day, and how long does a buyer spend on one today?
- A. Small: under ~100/day
- B. Medium: ~100 to 2,000/day
- C. Large: more than 2,000/day
Or tell me in your own words.

---

**User:** C. About 400 buyers, each gets ~30 supplier e-mails a day, so about 12,000 a day. A buyer spends ~5 minutes per e-mail today.

---

**AI:** **Q2 — Users.** How many people will use it, and how?
- A. A few power users (≤ ~10, all day)
- B. Many average users (a few requests a day each)
- C. Occasional users, or none (runs in the background)
Or tell me in your own words.

---

**User:** B. The 400 buyers, each a few times an hour: an average user, not all day.

---

**AI:** **Q3 — Data sensitivity.** Can these e-mails leave the company (sent to a cloud AI provider under a business contract)?
- A. Yes, ordinary business data
- B. Mixed: some sensitive, some not
- C. No, they must stay inside
Or tell me in your own words.

---

**User:** C. They contain negotiated prices and supplier negotiations: they must stay inside the company.

---

**AI:** **Q4 — Difficulty.** How hard is the task?
- A. Repetitive and standard (extract fields, fill a template)
- B. Medium (summarize, draft with some judgment)
- C. Hard, multi-step (an "agent" using tools, planning)
If it is a mix, say roughly what share is hard. Or tell me in your own words.

---

**User:** B. Medium: summarize and draft with some judgment.

---

**AI:** **Q5 — Duration.** How long will you need it?
- A. One-off / a project of a few weeks or months
- B. Permanent (part of the daily process)
- C. Not sure yet / pilot first
Or tell me in your own words.

---

**User:** B. Permanent.

---

**AI:** **Q6 — Checked examples.** Do you have e-mails where the correct extraction or reply is known? They are used to test models and to tune a small model.
- A. None
- B. A few dozen (around 50)
- C. Hundreds or more
Or tell me in your own words.

---

**User:** B. A few dozen e-mails with checked extractions.

---

**AI:** **Q7 — Internal skills.** Who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a workstation or a server
- C. We have data-science, AI / ML or platform engineers
Or tell me in your own words.

---

**User:** B. IT can install and maintain servers; we have no ML team.

---

**AI:**

**Your case in one line:** supplier e-mails (extract date/price changes + draft replies), ~12,000/day, 400 average users, confidential (must stay inside), medium difficulty, permanent.

**1. Recommended deployment: Open model on a shared on-site server**
- Negotiated prices must stay inside (Q3 = C, R1) → no frontier cloud AI, no rented/hosted GPUs.
- 400 buyers using it a few times an hour (Q2 = B, R4) → one shared server (some queueing at peaks), not a machine per person.
- IT can run servers (Q7 = B, R8) → an on-site server is realistic. 12,000 e-mails/day and 400 users fit one 8-GPU server (rough guide: tens of thousands of documents/day, 200–1,000 average users).
- Concretely: one 8-GPU server in your data center running a large open model (an "open-weight" model you can host yourself), connected to the mail system; each buyer sees the extracted date/price changes and a draft reply, then edits and sends.
- R5 (small tuned model) and R7 (routing) did not fire: the task is medium, not mostly standard, and nothing may leave the company.

**2. Alternatives**
- **Workstation per user** — main drawback: 400 machines ≈ $3.2–6.4 M up front and 400 boxes to maintain; does not scale to hundreds of people.
- **Routing, fully on site** (small tuned model for the field extraction, big model for the drafts) — main drawback: more moving parts and an integrator needed for tuning (R8); worth it only if tests show extraction is a large, standard share.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $250,000 | $550,000 |
| One-off backlog, if any (AI + review) | none | none |
| Running, per month (AI) | $15,000 | $17,000 |
| Human review of flagged items, per month | $330,000 | $330,000 |
| Total per year (AI + human review) | $4.1 M | $4.2 M |
| 3-year total | $13 M | $13 M |
| Today's manual cost, per year (if known) | $16 M | |

Assumptions:
- Volume: 12,000 e-mails/day (your figure) × 21 working days = 252,000/month; × 252 days = 3,024,000/year.
- Setup = server $220,000–450,000 + integration (mail system, test, train buyers) $30,000–100,000. No tuning included.
- Running = power, cooling, rack $2,000–4,000/month + 1 full-time person $12,500/month = $14,500–16,500/month. No hardware amortization (already in setup); for comparison, the server spread over 5 years ≈ $3,700–7,500/month.
- Human review: the AI drafts replies the buyer sends, so a buyer reads **every** e-mail in ¼ of today's 5 min (1.25 min) → 252,000 × 1.25 min = 5,250 h/month × $63/h (reference figure, your salary not given) = $330,750/month.
- Total per year = ($14,500–16,500 + $330,750) × 12 = $4,143,000–4,167,000.
- 3-year total = setup + 36 × monthly = $250,000 + $12,429,000 = $12,679,000 to $550,000 + $12,501,000 = $13,051,000 (≈ $12.7–13.1 M).
- Today: 3,024,000 e-mails/year × 5 min = 252,000 h × $63 = $15,876,000/year.
- Prices are November 2026 orders of magnitude; check them with suppliers.

**4. Before you decide: test on ~50 checked examples.** Run your few dozen checked e-mails (topped up to ~50 by a buyer if needed: about 1–2 days of work) through the on-site open model and one feasible alternative (e.g. a small tuned model for the extraction only, or another open model on the same server); compare accuracy, time per e-mail and cost per e-mail; check what goes wrong on the misses. Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes. One question per message, A/B/C options each ending with "Or tell me in your own words.", 7 questions (the maximum; the first message answered none of them), no "I don't know" needed; result uses the exact headings, the table (backlog row "none"), the assumptions, step 4 and the closing offer.
- **Arithmetic correct:** checked with Python. 12,000 × 21 = 252,000/month; × 1.25 min = 5,250 h × $63 = $330,750; yearly ($14,500–16,500 + $330,750) × 12 = $4,143,000–4,167,000; 3-year $12,679,000–13,051,000; today 12,000 × 252 × 5 min = 252,000 h × $63 = $15,876,000. Monthly and yearly figures now use the same 252-day calendar.
- **Recommendation coherent:** yes. R1 removes cloud and rented GPUs, R4 picks the shared server, R8 allows it with IT; volume and users fit the capacity guide. The "review every draft" rule makes human review the dominant cost (≈ 96% of running cost), which is realistic: the saving vs today (≈ $16 M → ≈ $4.1–4.2 M/year) comes from the buyer's time, not from the AI bill.
- **Remaining defect 1 (minor):** rounding to 2 significant digits collapses the 3-year range to "$13 M – $13 M" (real $12.7–13.1 M), and the review line has no range at all (one fixed share and time); the prompt could allow 3 digits when the range would vanish, or give a review-time range (e.g. ⅕–⅓ of today's time).
- **Remaining defect 2 (minor):** the table row label "Human review of flagged items" does not fit drafting cases where every item is read (kept as written here, although every draft is reviewed, not just flagged ones); and Q4 accepts one letter for a task that mixes standard extraction with medium drafting, so R5/R7 only fire if the user volunteers a share.
