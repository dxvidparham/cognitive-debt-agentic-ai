# Mitigating Flow State Disruption

> Failure mode: [05-flow-state-disruption](../failure-modes/05-flow-state-disruption.md)

Flow requires being the agent of the work. Supervising an agent is not flow — it's a different cognitive mode entirely. The mitigation is not to use AI less. It's to protect the conditions that make flow possible alongside AI use: uninterrupted generative time, deliberate supervision windows, and the solve-first habit that keeps you the author rather than the reviewer.

---

## Baseline

### Protect generative time

Schedule blocks where you write code without agent assistance. Not because the agent is bad, but because the generative work is what builds the mental model, the skill, and the satisfaction.

This isn't anti-AI. It's recognising that the agent is a tool for throughput, not a replacement for the cognitive work that makes you good at your job.

### Solve-first protocol

Before delegating any non-trivial task to an agent, spend 5–10 minutes sketching the solution yourself. Pseudocode, a rough outline, a test case list — anything that forces you to engage with the problem before handing it off.

Two effects: you build the mental model that lets you review the agent's output properly, and you preserve the generative engagement that creates flow.

### Batch supervision, protect flow windows

Treat agent supervision as scheduled work, not an interrupt. Set a timer. Review outputs in batches. Don't let the agent's pace dictate yours.

The goal is windows of uninterrupted generative work — even if those windows are shorter than they used to be.

### Rehash loop escape

If the agent keeps returning variations of the same broken answer despite prompt adjustments, stop prompting. The agent ran out of context — more tweaking will not help.

Protocol: (1) stop, (2) do independent research or read docs, (3) reframe the problem from scratch, (4) start a new session. Treat the rehash loop as a signal to think, not a signal to prompt harder.

This matters for flow because the rehash loop is a flow killer — it locks you into reactive mode, chasing a moving target, with no sense of progress or agency.

---

## AGENTS.md snippet

```markdown
## Solve-first protocol

Before delegating any non-trivial task to an AI agent:
1. Spend at least 5 minutes attempting the problem yourself
2. Write down your approach (pseudocode, outline, or test cases)
3. Only then delegate — with your approach as context in the prompt

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

> ⚗️ These are tools built by one developer to operationalise the baseline above. They are not prescriptions.

### `grill-me` skill

**What it does:** Before writing any implementation code, the agent interrogates you with targeted questions — one at a time — until your understanding of the solution is explicit and generative. The agent does not write code until you have passed a gate condition.

**Why it matters for flow:** The interrogation is the solve-first protocol in structured form. It forces the generative engagement that creates flow before handing off to the agent. You arrive at the delegation having already done the thinking — which means you can review the output from a position of understanding rather than discovery.

**Source:** Matt Pattlock. Adapted for OpenCode.

---

### `author-not-reviewer` skill

**What it does:** Authorship gate. The developer writes the load-bearing parts first — function signatures, data structures, key invariants. The agent fills in the elaboration. The human stays on the downbeat.

**Why it matters for flow:** Authorship is the condition for flow. When the agent writes everything, the developer's role shifts to reviewer — a mode that doesn't produce flow. This skill keeps the developer in the authoring role for the parts that matter.

**Source:** Custom.

---

### Rehash loop escape (global AGENTS.md policy)

**What it does:** A named protocol in the global AGENTS.md that the agent applies to itself when it detects it's in a rehash loop. The agent stops, acknowledges the loop, and recommends the escape protocol rather than continuing to generate variations.

**Status:** Active in global AGENTS.md. Text:

```markdown
**Rehash loop escape:** If AI keeps returning variations of the same broken answer
despite prompt adjustments, stop prompting. The AI ran out of context — more tweaking
will not help. Protocol: (1) stop, (2) do independent research or read docs,
(3) reframe the problem from scratch, (4) start a new session.
Treat the rehash loop as a signal to think, not a signal to prompt harder.
```

**Source:** Custom.

---

### Session budget cap (global AGENTS.md policy)

**What it does:** Sessions break at 50 messages. At message 45, the agent warns and wraps up with a session summary. Long sessions compound context costs and degrade output quality — which means more supervision work, more rehash loops, and less flow.

**Why it matters:** Knowing a session has a hard limit changes how you use it. You scope more tightly, delegate more deliberately, and don't let a session sprawl into a supervision marathon.

**Status:** Active in global AGENTS.md.

---

→ Back: [Mitigation index](./README.md) · Failure mode: [05-flow-state-disruption](../failure-modes/05-flow-state-disruption.md)
