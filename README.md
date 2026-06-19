# Cognitive Debt in the Age of Agentic AI

Resources for the lightning talk **"Cognitive Debt in the Age of Agentic AI"** by David, Brick Recognition & Sorting — The LEGO Group.

---

## What's in this repo

| Resource | Description |
|---|---|
| [`presentation/`](./presentation/) | Full presentation deck (PDF) |
| [`failure-modes/`](./failure-modes/) | All 6 failure modes — including the 3 not covered in the talk |
| [`research/`](./research/) | Source research files with evidence grading |
| [`mitigation/`](./mitigation/) | Bonus mitigation strategies and AGENTS.md snippets |
| [`sources.md`](./sources.md) | Annotated bibliography — all cited studies with links |

---

## The 3 failure modes covered in the talk

| Mode | One-liner | The fix |
|---|---|---|
| **Opacity** | You ship code you don't understand | Can't explain it to a colleague? Don't merge it. |
| **Trust Miscalibration** | Your confidence in AI output exceeds its actual reliability | Measure cycle time. Don't just trust the feeling. |
| **Skill Atrophy** | Delegation erodes the skills you stop practicing | Solve it yourself first. Then verify with AI. |

---

## The 3 additional failure modes (in the repo)

- **Accountability Gap** — when AI-generated code fails, responsibility diffuses
- **Context-Switching / AI Brain Fry** — managing multiple agents overwhelms working memory
- **Flow State Disruption** — supervisory work is not the same as deep work

---

## The causal chain

These aren't independent problems. They form a reinforcing feedback loop:

```
Opacity → Accountability → Trust → Context-Switching → Flow → Atrophy → (back to Opacity)
```

Mitigating comprehension (Opacity) is upstream of all other modes.

---

## Key sources

- **GitHub RCT** (arXiv:2302.06590) — 55% faster task completion, 95 developers
- **Longitudinal study** (arXiv:2509.19708) — 300 engineers, 1 year, 40% of production code is AI-written
- **METR RCT 2025** — developers felt 20% faster, were 19% slower
- **Anthropic RCT 2025** — AI use leads to 17% lower comprehension scores; biggest gap: debugging
- **Stack Overflow 2025** (49k respondents) — 84% adoption, only 3% highly trust AI output accuracy
- **arXiv:2605.23135** — worsened developer experiences nearly doubled (14% → 27%)

Full annotated bibliography → [`sources.md`](./sources.md)

---

## License

Research synthesis and presentation materials © 2025 The LEGO Group. Source studies retain their original licenses — see [`sources.md`](./sources.md) for links.
