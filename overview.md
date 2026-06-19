# Cognitive Debt in the Age of Agentic AI

AI is making developers faster. That part is real and measurable. But speed has a hidden cost — one that doesn't show up in sprint velocity or PR counts until it's already compounded.

This repo is the companion resource for the lightning talk of the same name. It covers the research behind the talk, the three failure modes in depth, and concrete fixes you can apply this week.

---

## The good news is real

Before anything else: the productivity gains from AI tools are genuine and well-documented.

- **55% faster task completion** — GitHub RCT, 95 professional developers ([arXiv:2302.06590](https://arxiv.org/abs/2302.06590))
- **32% faster PR review cycle times** — longitudinal study, 300 engineers, 1 year ([arXiv:2509.19708](https://arxiv.org/html/2509.19708v1))
- **40% of production code is now AI-generated** — same study, August 2025 peak
- **84% of developers use AI tools** — Stack Overflow 2025, 49,000+ respondents ([survey.stackoverflow.co/2025](https://survey.stackoverflow.co/2025))

This isn't hype. These are controlled studies with real sample sizes. AI tools genuinely help.

---

## So what's the problem?

The same longitudinal study that found 32% faster PR reviews also found that worsened developer experiences **nearly doubled** — from 14% to 27% — over the same period.

Developers are shipping faster. And quietly, something is eroding underneath.

The METR Research team ran a controlled trial in 2025 with 16 experienced open-source developers across 246 real tasks. The result: **AI made them 19% slower** — while they *believed* they were 20% faster. A 39-percentage-point gap between perception and reality. ([metr.org](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/))

This is the AI paradox: measurable gains coexisting with measurable hidden costs. The hidden costs have a name.

---

## Cognitive debt

Cognitive debt is what accumulates when AI handles the execution while the human handles the approval. The code ships. The understanding doesn't.

It's not one problem. It's a **feedback loop** of six interconnected failure modes:

```
Opacity → Accountability → Trust → Context-Switching → Flow → Atrophy → (back to Opacity)
```

Each mode feeds the next. Fixing comprehension (Opacity) is upstream of all the others.

This talk and repo cover the three most immediately relevant modes for working developers:

| Failure Mode | One-liner |
|---|---|
| [**Opacity**](./failure-modes/opacity.md) | You ship code you don't understand |
| [**Trust Miscalibration**](./failure-modes/trust-miscalibration.md) | Your confidence in AI output exceeds its actual reliability |
| [**Skill Atrophy**](./failure-modes/skill-atrophy.md) | Delegation erodes the skills you stop practicing |

---

## The three modes not covered in the talk

The full loop has six modes. The repo covers all of them eventually. The three not in the talk:

**Accountability Gap** — when AI-generated code fails in production, responsibility diffuses. The tool can't be accountable. The human who prompted it didn't write it. The reviewer rubber-stamped a diff. Ownership evaporates at exactly the moment it's needed.

**Context-Switching / AI Brain Fry** — agentic tools don't just generate code, they return large multi-file diffs that require the developer to context-switch out of deep work, parse what changed, decide whether to accept, then re-enter the original mental model. 66% of developers report spending more time fixing "almost-right" AI code (Stack Overflow 2025). That's a structural context-switch tax on every agent invocation.

**Flow State Disruption** — flow requires you to be the agent of the work: to struggle, make micro-decisions, hold the whole problem in mind. Delegating to an AI removes the struggle. The developer becomes a supervisor rather than a craftsperson. A 2025 Hacker News thread titled "Lost the Joy of Programming" accumulated hundreds of responses from experienced developers describing exactly this shift. ([HN:44499063](https://news.ycombinator.com/item?id=44499063))

---

## The systematic fix

The goal isn't to stop using AI. It's to change which parts you delegate.

Not all friction is equal. Bob Belderbos (2026) offers a useful taxonomy:

| Friction type | Examples | What to do |
|---|---|---|
| **Worth deleting** | Boilerplate, config syntax, scaffolding | Delegate freely |
| **Worth keeping** | Debugging, design decisions, code review | Preserve — this is where instinct forms |
| **Worth seeking out** | Reading unfamiliar internals, tracing below your abstraction level | Actively resist delegation |

The skills that atrophy fastest are exactly the ones in the "worth keeping" and "worth seeking out" categories — because those are the ones AI is most capable of handling, and therefore the ones developers are most tempted to hand off.

Three guardrails — one per failure mode:

| Failure Mode | Guardrail |
|---|---|
| Opacity | **Comprehension gate** — can't explain it to a colleague? Don't merge it. |
| Trust Miscalibration | **Measure, don't feel** — track actual cycle time. Don't trust the feeling of speed. |
| Skill Atrophy | **Solve-first protocol** — attempt the problem yourself before opening the chat. |

---

## What's in this repo

```
failure-modes/
  opacity.md               — You ship code you don't understand
  trust-miscalibration.md  — Your confidence exceeds AI's actual reliability
  skill-atrophy.md         — Delegation erodes the skills you stop practicing

research/                  — Source research files (coming)
sources.md                 — Annotated bibliography with all cited studies
```

---

## Sources

All claims in this repo are sourced. The three primary studies cited throughout:

- **METR RCT (2025)** — [metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)
- **GitHub/Peng RCT (2023)** — [arXiv:2302.06590](https://arxiv.org/abs/2302.06590)
- **Longitudinal enterprise study (2025)** — [arXiv:2509.19708](https://arxiv.org/html/2509.19708v1)
- **Anthropic coding skills RCT (2025)** — [anthropic.com/research/AI-assistance-coding-skills](https://www.anthropic.com/research/AI-assistance-coding-skills)
- **Stack Overflow Developer Survey 2025** — [survey.stackoverflow.co/2025](https://survey.stackoverflow.co/2025)

Full annotated bibliography → [sources.md](./sources.md) *(coming)*
