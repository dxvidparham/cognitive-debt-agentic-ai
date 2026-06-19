<div align="center">

# Cognitive Debt in the Age of Agentic AI

**AI is making developers faster. That part is real. But speed has a hidden cost.**

[![License](https://img.shields.io/badge/license-CC%20BY%204.0-blue?style=flat-square)](https://creativecommons.org/licenses/by/4.0/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](http://makeapullrequest.com)

</div>

---

The same longitudinal study that found **32% faster PR review cycles** also found that worsened developer experiences **nearly doubled** — from 14% to 27% — over the same period. The METR RCT found developers felt **20% faster** while actually being **19% slower**.

Developers are shipping faster. And quietly, something is eroding underneath.

This repo is the companion resource for the lightning talk *"Cognitive Debt in the Age of Agentic AI"*. It covers the research behind the talk, all six failure modes in depth, and concrete fixes you can apply this week.

---

## The causal chain

These aren't independent problems. They form a reinforcing feedback loop:

```
Opacity → Accountability Gap → Trust Miscalibration → Context Switching → Flow Disruption → Skill Atrophy → (back to Opacity)
```

Each mode feeds the next. Fixing comprehension (Opacity) is upstream of all the others.

---

## The six failure modes

| # | Mode | One-liner | Read |
|---|------|-----------|------|
| 01 | **Opacity** | You ship code you don't understand | [failure-mode](./01-opacity/failure-mode.md) · [mitigation](./01-opacity/mitigation.md) |
| 02 | **Accountability Gap** | Responsibility diffuses when AI-generated code fails | [failure-mode](./02-accountability-gap/failure-mode.md) · [mitigation](./02-accountability-gap/mitigation.md) |
| 03 | **Trust Miscalibration** | Your confidence in AI output exceeds its actual reliability | [failure-mode](./03-trust-miscalibration/failure-mode.md) · [mitigation](./03-trust-miscalibration/mitigation.md) |
| 04 | **Context Switching** | Managing agents overwhelms working memory | [failure-mode](./04-context-switching/failure-mode.md) · [mitigation](./04-context-switching/mitigation.md) |
| 05 | **Flow State Disruption** | Supervising an agent is not the same as writing code | [failure-mode](./05-flow-state-disruption/failure-mode.md) · [mitigation](./05-flow-state-disruption/mitigation.md) |
| 06 | **Skill Atrophy** | Delegation erodes the skills you stop practicing | [failure-mode](./06-skill-atrophy/failure-mode.md) · [mitigation](./06-skill-atrophy/mitigation.md) |

Modes 01, 03, and 06 were covered in the talk. All six are in the repo.

---

## Quick start

**If you have 5 minutes:** Read [overview.md](./overview.md) — the full picture in one page.

**If you want the fixes now:** [agents-md-snippets.md](./agents-md-snippets.md) — all six AGENTS.md blocks, ready to copy.

**If you want the evidence:** [sources.md](./sources.md) — annotated bibliography with links to every cited study.

---

## Three guardrails — one per talk failure mode

| Failure Mode | Guardrail |
|---|---|
| Opacity | **Comprehension gate** — can't explain it to a colleague? Don't merge it. |
| Trust Miscalibration | **Measure, don't feel** — track actual cycle time. Don't trust the feeling of speed. |
| Skill Atrophy | **Solve-first protocol** — attempt the problem yourself before opening the chat. |

---

## Key evidence

- **GitHub RCT** ([arXiv:2302.06590](https://arxiv.org/abs/2302.06590)) — 55% faster task completion, 95 professional developers
- **Longitudinal study** ([arXiv:2509.19708](https://arxiv.org/html/2509.19708v1)) — 300 engineers, 1 year; worsened DX nearly doubled (14% → 27%)
- **METR RCT 2025** ([metr.org](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)) — felt 20% faster, were 19% slower; 39-point perception gap
- **Anthropic RCT 2025** ([anthropic.com](https://www.anthropic.com/research/code-comprehension)) — 17% lower comprehension scores; biggest gap: debugging
- **Stack Overflow 2025** ([survey.stackoverflow.co/2025](https://survey.stackoverflow.co/2025)) — 84% adoption, only 3% highly trust AI output accuracy

Full annotated bibliography → [sources.md](./sources.md)

---

## What's in this repo

```
01-opacity/                — You ship code you don't understand
02-accountability-gap/     — Responsibility diffuses when AI code fails
03-trust-miscalibration/   — Your confidence exceeds AI's actual reliability
04-context-switching/      — Managing agents overwhelms working memory
05-flow-state-disruption/  — Supervisory work is not deep work
06-skill-atrophy/          — Delegation erodes the skills you stop practicing

agents-md-snippets.md      — All six AGENTS.md blocks in one place
overview.md                — Full picture in one page
sources.md                 — Annotated bibliography
```

Each mode directory contains `failure-mode.md` (what it is, why it happens, evidence, costs) and `mitigation.md` (baseline fixes, AGENTS.md snippet, experimental tools).

---

## License

Research synthesis and presentation materials © 2025 The LEGO Group. Source studies retain their original licenses — see [sources.md](./sources.md) for links.
