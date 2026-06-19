# Mitigating Trust Miscalibration

> Failure mode: [03-trust-miscalibration](../failure-modes/03-trust-miscalibration.md)

Trust miscalibration is insidious because it feels like confidence. The fix is not to trust less — it's to replace felt confidence with measured confidence. Distrust fluency. Verify claims. Give security no benefit of the doubt.

---

## Baseline

### Distrust fluency by default

Fluent code is not correct code. AI-generated code is optimised for plausibility, not correctness. The more natural it reads, the more dangerous it is to accept without verification — because fluency suppresses the instinct to check.

The rule: fluency is a reason to look more carefully, not less.

### Measure, don't feel

"This looks right" is not a verification. For any non-trivial output, run it:

- Does it pass the existing test suite?
- Does it handle the edge cases you can enumerate?
- Does it behave correctly on a real input, not just the happy path?

If you can't run it, write a test that would catch the most likely failure mode. The act of writing the test forces you to model the code's behaviour — which is the verification.

### Security gets no benefit of the doubt

The Stanford study found AI-assisted developers produced less secure code in 4 out of 5 security-relevant tasks, while rating their code as more secure than the control group. The confidence-competence inversion is most dangerous in security because the cost of being wrong is highest.

Rule: any code touching auth, crypto, permissions, input validation, or external API contracts gets independent review. Not "I'll review it carefully." A second human who wasn't in the room.

### Stale knowledge check

AI models have training cutoffs. Library APIs change. Security best practices evolve. When implementing anything framework-specific or security-sensitive, verify the pattern against current documentation before accepting it.

---

## AGENTS.md snippet

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

## Experimental tools

> ⚗️ These are tools built by one developer to operationalise the baseline above. They are not prescriptions.

### `doubt-driven-development` skill

**What it does:** Adversarial cross-examination of non-trivial decisions before they stand. The agent takes the role of a sharp-edged adversary — a malicious developer, a lazy reviewer, a confused future maintainer — and stress-tests the decision from each angle. Triggers automatically on any claim of the form "I'm confident this is correct", "this is safe to deploy", "this is backwards-compatible".

**Why it matters:** Trust miscalibration is a confidence problem. This skill makes confidence earn its keep.

**Source:** Custom.

---

### `verification-before-completion` skill

**What it does:** Evidence gate before any success claim. The agent cannot say "done", "fixed", "passing", or create a commit/PR without first running the build, tests, and linter and confirming exit code 0. A "Bug fixed in production" claim additionally requires Sentry evidence.

**Core rule:** No success claim without evidence.

**Targets:** The gap between "I think this works" and "I have verified this works."

**Source:** Adapted from superpowers/verification-before-completion.

---

### `ce-compound` + `ce-compound-refresh` skills

**What they do:** Stale-knowledge refresh before implementing anything framework-specific. `ce-compound` runs a full staleness audit — identifies which libraries haven't been touched in 30+ days, checks for major version releases, grounds the implementation in current documentation. `ce-compound-refresh` is the targeted version for when a specific dependency has just been upgraded.

**Why it matters:** AI training cutoffs mean the model's knowledge of library APIs is always somewhat stale. The more confident the model sounds, the more dangerous this is — fluency masks staleness.

**Source:** Custom.

---

### Pre-commit hooks: semgrep + ripsecrets

**What they do:**

- `semgrep p/python + p/security-audit` — Python security patterns, runs before every commit
- `semgrep p/trailofbits` — ML/AI-specific security rules (8 baseline findings documented)
- `ripsecrets` — scans for hardcoded secrets before commit

**Why it matters:** Automated gates catch the class of trust failures that are systematic — the same wrong pattern appearing repeatedly in AI-generated code. They don't require the developer to remember to check.

**Status:** Running via `prek` pre-commit hooks. Config: `.pre-commit-config.yaml`.

**Source:** [semgrep](https://semgrep.dev), [ripsecrets](https://github.com/sirwart/ripsecrets), [Trail of Bits ruleset](https://github.com/trailofbits/semgrep-rules).

---

→ Back: [Mitigation index](./README.md) · Failure mode: [03-trust-miscalibration](../failure-modes/03-trust-miscalibration.md)
