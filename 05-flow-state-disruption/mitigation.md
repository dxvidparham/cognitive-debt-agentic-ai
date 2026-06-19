# Mitigating Flow State Disruption

> Failure mode: [failure-mode.md](./failure-mode.md)

The core problem is structural: agentic tools replace generative work with supervisory work, and supervision doesn't produce flow. The mitigation isn't to stop using agents — it's to protect the conditions that make flow possible, and to be deliberate about which cognitive work you keep for yourself.

---

## Baseline

### Solve-first protocol

Before delegating any non-trivial task, spend 5–10 minutes attempting it yourself. Pseudocode, a rough outline, a test case list — anything that forces you to engage with the problem before handing it off.

Two effects: you build the mental model that lets you review the agent's output properly, and you preserve the generative engagement that creates flow. You don't have to finish. The attempt is the point.

### Batch supervision, protect flow windows

Treat agent supervision as scheduled work, not an interrupt. Set a timer. Review outputs in batches. Don't let the agent's pace dictate yours.

The goal is windows of uninterrupted generative work — even if those windows are shorter than they used to be. Research on knowledge work consistently shows that deep focus requires 20–30 minutes of uninterrupted work to establish. An agent that surfaces for review every few minutes resets that clock every time.

### Scope the agent tightly

The larger the diff, the longer the review, and the more disruptive the context switch. Before starting any agent session, write down exactly what you want it to do and what files it should touch. A well-scoped agent returns a reviewable diff. An under-scoped agent returns a sprawling change that fragments your session.

---

## AGENTS.md snippet

```markdown
## Supervision boundaries

- Do not surface for review more than once every [N] minutes unless blocked.
- Batch related changes into a single review checkpoint rather than requesting
  approval for each file individually.
- If you are uncertain about direction, ask ONE clarifying question before
  starting — not mid-task.
- Prefer completing a full logical unit before pausing, so reviews are
  meaningful rather than fragmentary.
```

---

## Experimental tools

> ⚗️ These are tools one developer built and runs in production to test whether the theory holds in practice. They are not prescriptions — they are included because real attempts at mitigation are more useful than abstract recommendations.

### `grill-me` skill

**What it does:** Forces pre-implementation interrogation before any code is written. The agent asks targeted questions one at a time until your understanding of the solution is explicit and generative. You cannot skip to implementation.

**Why it matters for flow:** The interrogation is itself generative work. By the time you've answered the questions, you've built the mental model — which means the subsequent implementation (whether you write it or the agent does) is grounded in your own understanding. The supervisory mode that follows is competent supervision, not passive approval.

**Targets:** The flow illusion — where you feel productive (reviewing diffs) while the generative cognitive work isn't happening.

**Source:** Custom.

---

### `cognitive-gym` skill

**What it does:** Diagnostic deliberate-practice protocol for skill atrophy from AI delegation. Triggers when the developer notices skills decaying, after a fully-delegated sprint, or when debugging feels harder than it used to.

**Why it matters for flow:** Flow requires that challenge slightly exceeds skill. If skills are atrophying, the challenge threshold for flow rises — and the work that used to produce flow no longer does. The cognitive gym skill addresses the skill side of that equation.

**Source:** Custom.

---

### Solve-first protocol in global AGENTS.md

**What it does:** A hard rule in the global AGENTS.md: for any skill domain not exercised independently in 30+ days, make one genuine attempt before delegating to AI. The attempt can be brief — a pseudocode sketch, a design outline, a test case list.

**Why it matters:** The goal is to keep the mental model alive, not to avoid AI. AI then accelerates what you already understand. This is the difference between flow-compatible AI use and flow-replacing AI use.

**Status:** Active in global AGENTS.md. Text:

```markdown
**Solve-first protocol:** For any skill domain you haven't exercised independently
in 30+ days, make one genuine attempt before delegating to AI. The attempt can be
brief — a pseudocode sketch, a design outline, a test case list. The goal is to
keep the mental model alive, not to avoid AI. AI then accelerates what you already
understand.
```

**Source:** Custom, informed by Csikszentmihalyi's flow model and Bainbridge (1983).

---

→ Back: [Overview](../overview.md) · Next: [06-skill-atrophy/mitigation.md](../06-skill-atrophy/mitigation.md)
