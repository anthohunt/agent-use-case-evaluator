# AI Use Case Evaluator

**How to use**
- **ChatGPT, Gemini, Copilot, any AI:** copy [prompt.md](prompt.md) into a new conversation, then describe your task.
- **Claude:** add the skill [skill/agent-use-case-evaluator](skill/agent-use-case-evaluator/SKILL.md), or simply copy [prompt.md](prompt.md).

---

## What it does

You describe **one task** you want AI to do. The evaluator asks **5 to 7 short questions, one at a time** (A / B / C or your own words):

1. **Volume** — how many items per day or month
2. **Users** — power, average or occasional
3. **Data sensitivity** — can it leave the company?
4. **Difficulty** — repetitive or hard multi-step
5. **Duration** — one-off or permanent
6. **Checked examples** — to test and tune a small model
7. **Internal skills** — who runs the hardware

Then you get:

1. **A recommended deployment**, among:
   - frontier cloud AI
   - open model on a workstation per user
   - open model on a shared on-site server
   - rented GPUs / hosted open model
   - **routing**: a small triage model sends easy items to a small model on site, sensitive hard items to a bigger open model on site, non-sensitive hard items to cloud AI
2. **Two alternatives** and their main drawback
3. **An estimated quote**: setup, then running cost per month and per year, with every assumption visible
4. **Next step**: test on ~50 checked examples before deciding

## Example: 14,000 supplier contracts

EXAMPLE_PLACEHOLDER

Full conversation: [examples/contracts.md](examples/contracts.md).
Other test runs: [supplier e-mails for 400 buyers](examples/supplier-emails.md) · [personal agent for 6 senior planners](examples/planner-agent.md).

## How it decides (readable rules)

- Data must stay inside → **no cloud**
- A few heavy users → **a workstation each**
- Many average users → **a shared on-site server** (some queueing at peaks)
- One-off need → **rent**, don't buy
- Repetitive task + checked examples → **a small tuned model**
- Very hard and not sensitive → **frontier cloud AI**
- Large, mixed workload → **routing** with a triage model

## Reference figures (November 2026, orders of magnitude)

| Item | Figure | Source / check |
|---|---|---|
| Workstation running an open model | ≈ $8,000 (Dell Pro Max with GB10, 128 GB) to ≈ $16,000 | Presentation. Retail GB10 price seen at ≈ $6,000 (US, Nov 2026); one RTX PRO 6000 Blackwell card alone lists ≈ $16,000, so a multi-GPU tower costs more. |
| On-site 8-GPU server | ≈ $220,000–450,000 + ≥ 1 full-time person, ~5-year life | Presentation. 8×H200 systems listed from ≈ $264,000; 8×B200 systems ≈ $300,000–500,000+. |
| 8 rented GPUs | ≈ $23,000–47,000 / month | Presentation (≈ $4–8 per GPU-hour). Oct 2026 market: ≈ $2.8–8.2 per GPU-hour depending on GPU and provider. |
| Frontier cloud AI vs cheap open model | ≈ $10 vs ≈ $0.60 per hard task (Claude Opus 5.5 vs DeepSeek V4.1 Flash) | Presentation, from the Artificial Analysis Intelligence Index. The gap is large in every published comparison. |
| Frontier cloud AI per standard document | ≈ $0.10 per contract | Presentation (contracts case). Consistent with ≈ $4–5 per million input tokens for a frontier model. |
| Tuning a small model for one task | ≈ 1 day, ≈ $26 of API | Presentation ("Kev" case, Session 3). |
| Manual contract review | 3 h and ≈ $190 per contract | Presentation (contracts case). |
| Staff to run a server | ≈ $150,000 / year loaded per full-time person | Assumption, adjust to your company. |

Prices move fast: check with suppliers before you buy.

> **These are estimates: test on ~50 checked examples before deciding.**

## Install the Claude skill

- **Claude apps (claude.ai, desktop):** download [agent-use-case-evaluator.zip](skill/agent-use-case-evaluator.zip), then upload it in Settings → Capabilities → Skills.
- **Claude Code:** copy the folder `skill/agent-use-case-evaluator` into `~/.claude/skills/`.
- Then just say: *"Evaluate this AI use case: …"*

## Files

- [prompt.md](prompt.md) — the full prompt, works in any AI
- [skill/agent-use-case-evaluator/SKILL.md](skill/agent-use-case-evaluator/SKILL.md) — the same logic as a Claude skill
- [examples/](examples/) — three complete test conversations

*From the talk "AI Economics, Model Routing & Deployment Trade-offs" — Anthony Hunt, Georgia Tech, November 2026.*
