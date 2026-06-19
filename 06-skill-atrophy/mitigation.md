# Mitigating Skill Atrophy

> Failure mode: [failure-mode.md](./failure-mode.md)

Skill atrophy is the slowest failure mode and the hardest to detect. You don't notice it until you need a skill you've stopped practicing — and by then, the gap is already there. The mitigation is not to delegate less. It's to delegate deliberately: preserve the friction that builds skill, and schedule the practice that keeps the mental model alive.

---

## Baseline

### Solve-first protocol

For any skill domain you haven't exercised independently in 30+ days, make one genuine attempt before delegating. The attempt can be brief — a pseudocode sketch, a design outline, a test case list. The goal is to keep the mental model alive, not to avoid AI.

AI then accelerates what you already understand. Without the attempt, you're delegating to a tool you can't verify.

### Deliberate practice windows

Schedule time each week to work on problems without AI assistance. Not production code — practice problems, side projects, or reimplementing something you already understand. The goal is to maintain the skills that agentic tools are most likely to atrophy: debugging, architectural reasoning, algorithmic thinking.

Bainbridge's irony applies directly: the more reliable the agent, the less practice you get, and the less capable you are when the agent fails and you need to take over.

### Protect the friction worth keeping

Not all friction is waste. The friction of debugging a hard problem builds the mental model of how the system actually works. The friction of writing a function from scratch builds the pattern library that makes future code faster to write and easier to review.

Before delegating a task, ask: is this friction I should be paying? If the task is in a domain you're trying to stay sharp in, the friction is the point.

### For managers: protect junior developers

Skill atrophy is most acute for developers who are still building their foundational skills. A junior developer who delegates everything to an agent before they've built the mental models that make delegation safe is not accelerating — they're skipping the foundation.

Protect time for junior developers to work without AI assistance on problems that are within their reach. The agent should accelerate their growth, not replace it.

---

## AGENTS.md snippet

```markdown
## Learning-oriented interaction rules

- When I ask "how do I do X?", explain the concept and give a generalized example
  in a DIFFERENT domain. Do not write the solution for my specific problem.
- When I share code I wrote, critique it and explain improvements rather than
  rewriting it for me.
- If I ask you to debug something, ask me what I think the problem is first
  before offering your analysis.
- For any algorithm or data structure you use, briefly explain why it was chosen
  over the obvious alternative.
```

---

## Experimental tools

> ⚗️ These are tools built by one developer to operationalise the baseline above. They are not prescriptions.

### `cognitive-gym` skill

**What it does:** Diagnostic deliberate-practice protocol. Surfaces which skills are at risk before they've degraded: delegation audit (what have you delegated in the last 30 days?), rep design (practice problem targeting the most atrophied skill), teach-one (explain the concept to a rubber duck or junior developer), scheduling (block time), gym log (record what was practiced and what gaps remain).

**Why it matters:** Skill atrophy is invisible until it's a problem. The delegation audit makes it visible. The gym log creates a record of what's being maintained and what isn't.

**Triggers:** "I used to be able to do this", "I always let the agent handle X", post-sprint retrospective, any explicit concern about skill decay.

**Source:** Custom.

---

### `skill-stub-prompter` skill

**What it does:** After any task with 5+ tool calls that follows a repeatable procedure, prompts to capture the procedure as a skill stub. Prevents procedural knowledge from staying implicit in session history.

**Why it matters for atrophy:** When you delegate a procedure repeatedly without capturing it, the procedure lives only in the agent's context — not in your head, not in a doc. The skill stub forces externalisation: you have to describe the procedure well enough to encode it, which is a form of deliberate practice.

**Source:** Custom.

---

### `task-outcome-logger` skill

**What it does:** After every completed task, logs 5 outcome variables to a structured JSONL file: verification pass rate, blast radius (files changed vs expected), retry count, plan deviation, audit burden. Over time, this builds a personal DevEx metrics baseline.

**Why it matters for atrophy:** Skill atrophy shows up in the metrics before it shows up in your self-assessment. Rising retry counts, increasing blast radius, declining verification pass rates — these are early signals that something is degrading. Without the log, you can't see the trend.

**Log location:** `~/.config/opencode/metrics/task-outcomes.jsonl`

**Source:** Custom.

---

### `majority-solution-check` (AGENTS.md policy)

**What it does:** A named policy in the global AGENTS.md. Before accepting any AI-generated algorithm, data structure, or architecture decision, the agent asks: "Is this the generic solution? What constraint would have changed the answer?"

**Why it matters for atrophy:** AI defaults to the most statistically common solution in its training data — not the most appropriate one for your context. Accepting majority solutions without questioning them atrophies the skill of recognising when context changes the answer. The check keeps that skill active.

**Status:** Active in global AGENTS.md. Text:

```markdown
**Majority solution check:** AI defaults to the most statistically common solution
in its training data, not the most appropriate one for your context. Before accepting
any AI-generated algorithm, data structure, or architecture decision, ask:
"Is this the generic solution? What constraint would have changed the answer?"
If you didn't specify the constraint, the AI didn't know to apply it.
```

**Source:** Custom.

---

→ Back: [Overview](../overview.md) · Failure mode: [failure-mode.md](./failure-mode.md) · Start over: [01-opacity/failure-mode.md](../01-opacity/failure-mode.md)
