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
If it is a mix, say roughly what share is hard.

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

---

**User:** B. A few dozen e-mails with checked extractions.

---

**AI:** **Q7 — Internal skills.** Who would run the hardware and the models?
- A. Nobody: we only buy software as a service
- B. Our IT team can install and maintain a server
- C. We have an AI / ML or platform engineering team

---

**User:** B. IT can install and maintain servers; we have no ML team.

---

**AI:**

**Your case in one line:** supplier e-mails (extract date/price changes + draft replies), ~12,000/day, 400 average users, confidential (must stay inside), medium difficulty, permanent.

**1. Recommended deployment: Open model on a shared on-site server**
- Negotiated prices must stay inside (Q3 = C, R1) → no frontier cloud AI, no rented/hosted GPUs.
- 400 buyers using it a few times an hour (Q2 = B, R4) → one shared server (some queueing at peaks), not a machine per person.
- IT can run servers (Q7 = B, R8) → an on-site server is realistic; ~12,000 e-mails/day is within what one 8-GPU server handles (tens of thousands of documents/day, 200–1,000 average users — rough guide).
- Concretely: one 8-GPU server in your data center running a large open model (an "open-weight" model you can host yourself), connected to the mail system; it shows each buyer the extracted changes and a draft reply, and the buyer edits and sends.
- R5 (small tuned model) did not fire, because the task is medium, not repetitive; your few dozen checked extractions are still enough for the test in step 4.

**2. Alternatives**
- **Workstation per user** — main drawback: 400 machines ≈ $3.2–6.4 M up front and 400 boxes to maintain; does not scale to hundreds of people.
- **Routing, fully on site** (small tuned model for the field extraction, big model for drafting) — main drawback: more moving parts and an integrator needed for tuning (R8); worth it only if tests show extraction alone is a large, standard share.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $250,000 | $550,000 |
| Running, per month (AI) | $15,000 | $17,000 |
| Human review of flagged items, per month | $33,000 | $66,000 |
| Total per year | $570,000 | $990,000 |
| 3-year total | $2.0 M | $3.5 M |
| Today's manual cost, per year (if known) | $16 M | |

Assumptions:
- Volume: 12,000 e-mails/day (your figure) × 21 working days = 252,000/month; 250 working days/year for today's cost.
- Setup = server $220,000–450,000 + integration (mail system, test, train buyers) $30,000–100,000. No tuning included.
- Running = power, cooling, rack $2,000–4,000/month + 1 full-time person $12,500/month = $14,500–16,500/month. No hardware amortization (already in setup); for comparison, the server spread over 5 years ≈ $3,700–7,500/month.
- Human review: 10–20% of e-mails flagged (25,200–50,400/month), each checked in ¼ of today's 5 min (1.25 min) → 525–1,050 h/month × $63/h (reference figure, your salary not given) = $33,075–66,150/month.
- Total per year = (AI + human review) × 12 = $47,575 × 12 = $570,900 to $82,650 × 12 = $991,800.
- 3-year total = setup + 36 × monthly = $250,000 + $1,712,700 = $1,962,700 to $550,000 + $2,975,400 = $3,525,400.
- Today: 12,000 × 250 = 3,000,000 e-mails/year × 5 min = 250,000 h × $63 = $15,750,000/year.
- Prices are November 2026 orders of magnitude; check them with suppliers.

**4. Before you decide: test on ~50 checked examples.** Run your few dozen checked e-mails (topped up to ~50 by a buyer if needed: about 1–2 days of work) through the on-site open model and one alternative (e.g. a small tuned model for the extraction only); compare accuracy, time per e-mail and cost per e-mail; check what goes wrong on the misses. Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes. One question per message, A/B/C options, 7 questions (the maximum), no question skipped because the first message gave none of the answers, no "I don't know" needed; result uses the exact headings, the table and the closing offer. Q6 and Q7 dropped the "Or tell me in your own words" line to stay within 4–6 lines (Q4 replaced it with the mix instruction).
- **Arithmetic correct:** checked. 12,000 × 21 = 252,000; 10–20% × 1.25 min = 525–1,050 h → $33,075–66,150; monthly $47,575–82,650 → yearly $570,900–991,800; 3-year $1,962,700–3,525,400; today 3,000,000 × 5 min = 250,000 h × $63 = $15,750,000 (= 400 buyers × 2.5 h/day, consistent with 30 e-mails × 5 min).
- **Recommendation coherent:** yes. R1 (stay inside) removes cloud and rented GPUs, R4 (many average users) picks the shared server, R8 allows it with IT; volume fits the server capacity guide. R5/R7 correctly did not fire.
- **Remaining defect 1 (material):** the default human-review rule (10–20% flagged, ¼ of time) suits extraction, not reply drafting: buyers will read every draft before sending. If every e-mail gets 1.25 min of review, the review line becomes ≈ 5,250 h ≈ $330,000/month (≈ $4 M/year), not $33,000–66,000. The prompt should ask, or assume all items are reviewed when the AI drafts outgoing messages.
- **Remaining defect 2 (minor):** "Total per year" does not say whether it includes the human-review line (assumed yes here); 21 days × 12 = 252 vs 250 days/year gives a small mismatch between monthly and today's yearly cost; a task that mixes standard extraction with medium drafting has no clean path to R5 unless the user volunteers a share; rule R10 is listed before R8.
