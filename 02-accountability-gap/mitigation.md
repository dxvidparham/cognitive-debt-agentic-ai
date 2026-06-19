# Mitigating the Accountability Gap

> Failure mode: [failure-mode.md](./failure-mode.md)

Accountability doesn't disappear when AI writes the code. It diffuses — spread thin across the prompter, the reviewer, the tool, and the team until nobody clearly owns it. The mitigation is structural: make ownership explicit before the code ships, not after something breaks.

---

## Baseline

### One named human per AI change

Every AI-generated change that ships needs exactly one accountable human attached to it. Not "the team." One person who reviewed it, understood it, and is prepared to defend it at 2am.

This is the minimum condition for accountability to mean anything.

### Provenance tagging

Tag commits and PRs to indicate which parts were AI-generated and which were human-authored. This is not about blame — it's about audit trails.

```
fix: normalise error response format [AI-assisted, reviewed by @name]
```

A reviewer seeing `[AI]` knows to check edge cases, majority-solution defaults, and security assumptions more carefully. Six months later, when something breaks, you need to know whether the relevant code was human-reasoned or AI-generated.

### Review for understanding, not just correctness

"LGTM" on a diff you didn't model is not a review. It's a signature on a blank cheque.

The bar: can you explain what this code does and why, to someone who wasn't in the room? If not, the review isn't done.

---

## AGENTS.md snippet

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

## Experimental tools

> ⚗️ These are tools one developer built and runs in production to test whether the theory holds in practice. They are not prescriptions — they are included because real attempts at mitigation are more useful than abstract recommendations.

### `canary` skill

**What it does:** After any deploy, establishes an unbroken accountability chain: named owner declaration → deploy record → behavioural assertions → observation window → structured rollback gate. The deploy is not declared complete until all five steps are done.

**Core principle:** A deploy without a canary record is a deploy without an owner.

**Targets:** The accountability gap at the moment code leaves the repository — exactly when ownership is most likely to diffuse.

**Source:** Garry Tan · [garrytan/gstack](https://github.com/garrytan/gstack/tree/main/canary) (adapted).

---

### `blast-radius-check` skill

**What it does:** After every agent task completes, runs `git diff --stat` and cross-references changed files against the task spec's expected files. Any file modified outside the expected scope is a blast radius violation — the session is resumed and the out-of-scope change is reverted before proceeding.

**Why it matters:** AI-generated changes have no inherent scope boundary. A single flawed reasoning step can modify dozens of files outside the intended scope. Without blast radius tracking, post-mortems cannot reconstruct what happened.

**Targets:** Scope creep as an accountability failure — changes that nobody explicitly authorised.

**Source:** Custom.

---

### OPA policy: `agent-guardrails.rego`

**What it does:** Open Policy Agent rules evaluated before every tool call. Current policies:

- Block reads of `.env` files (secrets must not enter agent context)
- Block writes to `models/` (ML artifacts are S3-managed, never hand-edited)
- Require confirmation before `git push`

**Why it matters:** Accountability requires that the agent cannot take certain actions without explicit human approval. OPA enforces this at the system level — the agent cannot bypass it by reasoning its way around a prompt instruction.

**Status:** Running in production on one codebase. Policy file: `~/.config/opencode/policies/agent-guardrails.rego`.

**Source:** Custom, using [Open Policy Agent](https://www.openpolicyagent.org).

---

### Project hooks: pre-PR gate + blast radius logging

**What it does:** A `PreToolUse` hook fires before any commit or PR creation. It runs a readiness gate (tests pass, blast radius verified, named owner present in the PR description) and logs the outcome to `~/.config/opencode/metrics/task-outcomes.jsonl`.

**Why it matters:** The gate is the last line of defence before AI-generated code enters the shared history. The log is the audit trail.

**Status:** Running on one project. Hook config: `.github/hooks/hooks.json`.

**Source:** Custom, using the [OpenCode hooks system](https://opencode.ai/docs/hooks).

---

→ Back: [Overview](../overview.md) · Next: [03-trust-miscalibration/mitigation.md](../03-trust-miscalibration/mitigation.md)
