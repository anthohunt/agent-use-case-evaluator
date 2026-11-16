# Example: supplier e-mails for 400 buyers

A full test conversation run with `prompt.md` (November 2026 prices; all figures are estimates).

---

**User:** I want AI to help our buyers with supplier e-mails: extract key info (delivery date changes, price changes) and draft replies.

---

**AI:** **Q1 — Volume.** How many supplier e-mails per day or per month? And roughly how long does a buyer take per e-mail today?
- A. Small: under ~100/day (≤ 2,000/month)
- B. Medium: ~100 to 2,000/day
- C. Large: more than 2,000/day, or a big backlog

Or tell me in your own words.

---

**User:** C. About 400 buyers, each gets ~30 supplier e-mails a day, so about 12,000 a day. A buyer spends ~5 minutes per e-mail today.

---

**AI:** **Q2 — Users.** I noted: 400 buyers will use it. How will each of them use it: heavily all day, a few times a day, or rarely?

---

**User:** A few times an hour each: an average user, not all day.

---

**AI:** **Q3 — Data sensitivity.** Can these e-mails leave the company (sent to a cloud AI provider under a business contract)?
- A. Yes, ordinary business data
- B. Mixed: some e-mails are sensitive, others are not
- C. No, they must stay inside (confidential prices, personal data…)

Or tell me in your own words.

---

**User:** C. They contain negotiated prices and supplier negotiations: they must stay inside the company.

---

**AI:** **Q4 — Difficulty.** How hard is the task?
- A. Repetitive and standard (extract fields, fill a template)
- B. Medium (summarize, draft with some judgment)
- C. Hard, multi-step (reasoning across many documents, tools, an "agent")

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

**AI:** **Q6 — Checked examples.** Do you have e-mails where the correct answer is known (e.g. extractions already checked by a buyer)? They are used to test models and to tune a small model.
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

**Your case in one line:** supplier e-mails (extract delivery-date/price changes + draft replies), ~12,000/day, 400 average users, confidential (must stay inside), medium difficulty, permanent.

**1. Recommended deployment: Open model on a shared on-site server**
- Negotiated prices must stay inside (Q3 = C, R1) → no frontier cloud AI, no rented/hosted GPUs.
- 400 buyers, a few times an hour each (Q2 = average users, R4) → one shared server (some queueing at peaks), not a machine per person.
- IT can install and maintain servers (Q7 = B, R8) → an on-site server is realistic; ~12,000 e-mails/day and 400 users fit one 8-GPU server (rough guide: tens of thousands of documents/day, 200–1,000 average users).
- Concretely: one 8-GPU server in your data center running a large open model (an "open-weight" model you host yourself), connected to the mail system; each buyer sees the extracted date/price changes and a draft reply, then edits and sends it.
- R5 (small tuned model) and R7 (routing) did not fire: the task is medium, not mostly standard, and no share of items may go to the cloud.

**2. Alternatives**
- **Open model on a workstation per user** — main drawback: 400 machines ≈ $3.2–6.4 M up front and 400 boxes to maintain; does not scale to hundreds of people.
- **Routing, fully on site** (small tuned model for the field extraction, bigger open model for the drafts) — main drawback: more moving parts and an integrator needed for the tuning (R8); worth it only if tests show extraction is a large, standard share.

**3. Estimated quote** (orders of magnitude, November 2026 prices)

| | Low | High |
|---|---|---|
| Setup (one-time) | $250,000 | $550,000 |
| One-off backlog, if any (AI + review) | none | none |
| Running, per month (AI) | $15,000 | $17,000 |
| Human review (flagged or all items), per month | $260,000 | $440,000 |
| Total per year (AI + human review) | $3.3 M | $5.5 M |
| 3-year total | $10 M | $17 M |
| Today's manual cost, per year (if known) | $16 M | |

Assumptions:
- Volume: 12,000 e-mails/day (your figure) × 21 working days = 252,000/month; × 252 days = 3,024,000/year.
- Setup = server $220,000–450,000 + integration (mail system, testing, training buyers) $30,000–100,000 = $250,000–550,000. No tuning, no integrator fee (your IT runs the server).
- Running (AI) = power, cooling, rack $2,000–4,000/month + 1 full-time person $12,500/month = $14,500–16,500/month. No hardware amortization (already in setup); for comparison, the server spread over 5 years ≈ $3,700–7,500/month.
- Human review: the AI drafts replies a buyer sends, so a buyer reads **every** e-mail in ⅕–⅓ of today's 5 min (1–1.7 min) → 4,200–7,000 h/month × $63/h (reference figure; your salary not given) = $264,600–441,000/month.
- Total per year = ($14,500 + $264,600) × 12 = $3,349,200 to ($16,500 + $441,000) × 12 = $5,490,000.
- 3-year total = $250,000 + 36 × $279,100 = $10,297,600 to $550,000 + 36 × $457,500 = $17,020,000.
- Today: 3,024,000 e-mails/year × 5 min = 252,000 h × $63 = $15,876,000/year.
- Prices are November 2026 orders of magnitude; check them with suppliers.

**4. Before you decide: test on ~50 checked examples.** Run your few dozen checked e-mails (topped up to ~50 by a buyer if needed: about 1–2 days of work) through the on-site open model and one feasible alternative (e.g. a small tuned model for the extraction only, or another open model on the same server); if you later tune a small model, keep these test e-mails out of its tuning set; compare accuracy, time per e-mail and cost per e-mail; check what goes wrong on the misses. Decide only then. *These are estimates.*

Want me to change an assumption (volume, salaries, prices) and redo the quote?

---

## Test check

- **Prompt followed:** yes. One question per message; 7 questions (the maximum; the first message answered none fully). Q2 was partly answered by the Q1 reply (400 buyers), so the AI wrote "I noted: …" and asked only the missing part as a short open question, as the prompt requires. Result uses the exact headings, table (backlog "none"), assumptions, step 4 and closing offer. No "I don't know" was needed.
- **Arithmetic correct:** checked with Python. 252,000 e-mails/month; review 4,200–7,000 h × $63 = $264,600–441,000/month; yearly $3,349,200–5,490,000; 3-year $10,297,600–17,020,000; today 252,000 h × $63 = $15,876,000/year; 400 workstations $3.2–6.4 M.
- **Recommendation coherent:** yes. R1 removes cloud and rented GPUs, R4 picks the shared server, R8 allows it with IT; volume and users fit the capacity guide. Human review dominates (≈ 95–96% of running cost); the saving vs today (≈ $16 M → ≈ $3.3–5.5 M/year) comes from buyers' time, not from the AI bill. The review range from the ⅕–⅓ rule fixes the earlier single-value review line.
- **Remaining defect 1 (minor):** the partial-answer rule turns Q2 into an open question without A/B/C, which conflicts with "each question has options A / B / C"; the prompt could say "keep the A/B/C options if they still help".
- **Remaining defect 2 (minor):** rounding $14,500 to 2 significant digits is ambiguous ($14,000 or $15,000 depending on half-up vs half-even); and Q4 takes one letter for a task that mixes standard extraction with medium drafting, so R5/R7 only fire if the user volunteers a share (the persona did not).
