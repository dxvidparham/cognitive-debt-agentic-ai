# AGENTS.md Snippets — All Six Failure Modes

All six AGENTS.md blocks in one place. Copy what applies to your setup.

Each snippet targets a specific failure mode. They are designed to be additive — you can include all six without conflicts.

---

## 01 — Opacity

*Forces the agent to explain its reasoning, not just produce output.*

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

## 02 — Accountability Gap

*Constrains blast radius and forces explicit change disclosure.*

```markdown
## Scope boundaries

- You may only modify files under: [list directories]
- You may NOT modify: [list protected paths]
- Production config files require explicit human approval before changes.
- Every file you modify must be listed in your response with a one-line
  explanation of what changed and why.
- Before any destructive operation (delete, overwrite, schema migration),
  state what you are about to do and wait for explicit confirmation.
```

---

## 03 — Trust Miscalibration

*Prevents overconfident security claims and flags high-risk code.*

```markdown
## Trust and verification rules

- For any security-sensitive code (auth, crypto, permissions, input validation),
  explicitly state: "This code requires independent security review before merging."
- Never claim code is "safe" or "secure." Describe what it does and what it does
  NOT protect against.
- When you are uncertain about correctness, say so explicitly rather than
  generating plausible-looking code.
- Flag any code that modifies: authentication, authorization, encryption,
  database schemas, or external API contracts.
```

---

## 04 — Context Switching

*Enforces atomicity and creates natural review checkpoints.*

```markdown
## Scope and atomicity rules

- Work on ONE file or ONE logical unit at a time unless explicitly told otherwise.
  Complete it fully before moving to the next.
- After completing each logical unit, pause and summarise what was done
  before proceeding. This creates natural review checkpoints.
- Never start a new task while a previous task's output is unreviewed.
- Maximum diff size per response: [N] files or [N] lines changed. If the task
  requires more, break it into sequential steps and wait for approval between each.
```

---

## 05 — Flow State Disruption

*Reduces unsolicited interruptions and enforces the solve-first protocol.*

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

## 06 — Skill Atrophy

*Shifts the agent from answer-giver to thinking partner.*

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

## Putting it together

A minimal combined AGENTS.md section covering all six failure modes:

```markdown
## Agentic cognitive hygiene

### Comprehension (Opacity)
- Never generate code without explaining why this approach was chosen.
- For functions >20 lines, summarise: inputs → outputs → side effects.
- After every non-trivial block: What this does / Why this approach / What could go wrong.

### Scope (Accountability)
- List every file modified with a one-line explanation of what changed and why.
- Before any destructive operation, state intent and wait for confirmation.

### Trust (Trust Miscalibration)
- Never claim code is "safe" or "secure" — describe what it does and does NOT protect against.
- Flag any changes to: auth, crypto, permissions, DB schemas, external API contracts.

### Atomicity (Context Switching)
- Work on ONE logical unit at a time. Summarise before moving to the next.
- If a task requires >N files, break into sequential steps with approval between each.

### Supervision (Flow State Disruption)
- Batch review checkpoints. Do not surface for review mid-task unless blocked.
- Ask ONE clarifying question before starting — not mid-task.

### Learning (Skill Atrophy)
- When asked "how do I do X?", explain the concept with a different-domain example first.
- When asked to debug, ask what I think the problem is before offering analysis.
```
