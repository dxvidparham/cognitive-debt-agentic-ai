# Mitigating Opacity

> Failure mode: [01-opacity](../failure-modes/01-opacity.md)

Opacity is the root of the causal chain. Fix it here and the downstream modes get easier. Let it slide and everything else compounds.

---

## Baseline

### The comprehension gate

Before accepting any AI-generated code, you must be able to answer three questions without looking at the code:

1. What does this do?
2. Why this approach and not the obvious alternative?
3. What would break if this were wrong?

If you can't answer all three, the review isn't done. This is not a slow-down — it's the minimum bar for ownership.

### Generation-then-comprehension

When you need to understand a piece of code the agent wrote, don't just read it. Re-derive it:

1. Delete the agent's implementation (or set it aside)
2. Write your own version from scratch — even a rough one
3. Compare the two

The act of generating forces comprehension in a way that reading does not. This is the generation effect: information you produce yourself is retained better than information you review.

### Author the hard parts

For any implementation that crosses a module boundary, touches security, or involves non-obvious logic: write the skeleton yourself. Define the function signatures, the data structures, the key invariants. Then hand it to the agent to fill in.

You stay the author of the load-bearing decisions. The agent handles the elaboration.

---

## AGENTS.md snippet

Add this to your AGENTS.md (or equivalent config file):

```markdown
## Comprehension requirements

- Never generate code without explaining WHY this approach was chosen over alternatives.
- For any function longer than 20 lines, include a plain-English summary of its
  contract: inputs → outputs → side effects.
- When modifying existing code, explain what the previous code did and why the
  new version is better.

## Explanation format

After every non-trivial code block, append:
> **What this does:** [one sentence]
> **Why this approach:** [one sentence]
> **What could go wrong:** [one sentence]
```

---

## Experimental tools

> ⚗️ These are tools built by one developer to operationalise the baseline above. They are not prescriptions. They are included because seeing what someone actually built is more useful than seeing what they recommend.

### `grill-me` skill

**What it does:** Before writing any implementation code, the agent interrogates you with targeted questions — one at a time — until your understanding of the solution is explicit and generative. The agent does not write code until you have passed a gate condition.

**Three phases:** Solution framing (what problem, what approach, what alternative rejected) → Edge case interrogation (what breaks, what's missing, what assumption is wrong) → Gate (can you implement this without the agent if you had to?).

**Targets:** Opacity at the root — restores the cognitive struggle that builds the mental model before code exists.

**Source:** Matt Pattlock. Adapted for OpenCode.

---

### `audit-context-building` skill

**What it does:** Before modifying any unfamiliar code, forces a structural analysis pass: entry point map, call trace, invariant map, mental model statement. The agent cannot begin editing until it has produced a written mental model.

**Targets:** The "illusion of understanding" — where code looks correct but the agent (or developer) has no real model of how it works.

**Source:** Adapted from Trail of Bits audit methodology.

---

### `author-not-reviewer` skill

**What it does:** Authorship gate. The developer writes the load-bearing parts first (function signatures, data structures, key invariants). The agent fills in the elaboration. Enforces the "hold the pulse" principle — the human stays on the downbeat.

**Mandatory for:** Auth, payments, concurrency, public APIs, architectural boundaries.

**Targets:** The review-is-weaker-than-authorship finding from the Anthropic RCT.

**Source:** Custom.

---

→ Back: [Mitigation index](./README.md) · Failure mode: [01-opacity](../failure-modes/01-opacity.md)
