# Mitigations

This directory documents practical responses to the six failure modes. It is split into two layers:

**Baseline** — things anyone can do today, with no tooling required. Mostly AGENTS.md snippets and behavioural protocols.

**Experimental** — tools one developer built and is running in production to test whether the theory holds in practice. These are marked with `> ⚗️ Experimental` throughout. They work for that developer. They may not work for you. They are included because real attempts at mitigation are more useful than abstract recommendations.

---

## What's here

| File | What it covers |
|------|----------------|
| [agents-md-snippets.md](./agents-md-snippets.md) | All 6 AGENTS.md blocks in one place — copy-paste ready |
| [01-opacity.md](./01-opacity.md) | Comprehension gate, authorship, explanation requirements |
| [02-accountability-gap.md](./02-accountability-gap.md) | Scope boundaries, named ownership, provenance tagging |
| [03-trust-miscalibration.md](./03-trust-miscalibration.md) | Trust rules, security gates, stale-knowledge refresh |
| [04-context-switching.md](./04-context-switching.md) | Atomicity rules, concurrency cap, memory persistence |
| [05-flow-state-disruption.md](./05-flow-state-disruption.md) | Supervision boundaries, solve-first, generative time |
| [06-skill-atrophy.md](./06-skill-atrophy.md) | Learning-oriented interaction, deliberate practice, metrics |

---

## How to read these

Each mitigation doc follows the same structure:

1. **Baseline** — the minimum viable response. No tooling. Works in any agent environment.
2. **AGENTS.md snippet** — the specific rules to add to your AGENTS.md (or equivalent config file). Extracted from the corresponding failure mode doc.
3. **Experimental tools** — skills, hooks, MCP servers, and policies one developer built to operationalise the baseline. Flagged clearly. Not prescriptions.

---

## On the experimental tools

The experimental tools documented here are OpenCode skills, hooks, and policies built by one developer ([@dxvidparham](https://github.com/dxvidparham)) as a personal attempt to close the loop between theory and practice.

They are included here because:
- Abstract advice ("review AI code carefully") is easy to ignore
- Concrete tooling forces the behaviour at the system level
- Seeing what someone actually built is more useful than seeing what they recommend

They are flagged as experimental because:
- They are not peer-reviewed
- They are optimised for one developer's workflow and tool stack (OpenCode + Claude)
- Some of them are months old and may already need revision
- The underlying research they respond to is itself recent and still being replicated

If you adapt any of them, treat them as starting points, not finished products.
