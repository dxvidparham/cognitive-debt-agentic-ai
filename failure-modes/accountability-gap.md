# Accountability Gap — When AI-generated code fails, nobody owns it

> *"We didn't lose accountability. It's right there in the room with us. We just diffused it."*
>
> — Lance Dacy, Big Agile (2025)

---

## What it is

When a human writes code and it breaks in production, the chain of responsibility is clear. Someone made a decision. Someone can explain it. Someone gets paged.

Agentic AI breaks that chain.

The tool can't be accountable — it has no stake in the outcome. The developer who prompted it may not fully understand what it generated. The reviewer approved a diff without modelling the change. And when something goes wrong, everyone in that chain has a reasonable case for why it wasn't entirely their fault.

This is the accountability gap: ownership evaporates at exactly the moment it's needed.

---

## Why agentic tools make it worse

With traditional code, the gap between *intent* and *artifact* is small. You thought it, you typed it, you own it.

With agentic tools, that gap widens with every autonomous step the agent takes. A multi-step agent that plans, writes, tests, and modifies code across multiple files has made dozens of micro-decisions that no human explicitly approved. By the time the PR lands, the connection between the original prompt and the final code is long and indirect.

A real example from the research: an agent tasked with migrating API handlers autonomously standardised an inconsistent error response format. Technically correct. All local unit tests passed. But it silently broke integrations with external partners whose agreements existed nowhere in the codebase. No one person made that decision. No one person caught it. No one person was clearly responsible.

---

## The evidence

**Two-thirds of CIOs and CTOs are accountable for AI they can't fully control**

An IBM study found that 66% of senior technology leaders are held responsible for AI-driven system behaviours they cannot fully track or monitor. The accountability expectation hasn't changed. The visibility has.
→ [IBM/CIO study](https://www.cio.com/article/4182288/cios-are-being-held-accountable-for-ai-they-dont-fully-control-ibm-study-finds.html)

**DORA 2025 — delivery stability still negative**

Despite throughput recovering in 2025, AI adoption remains negatively associated with delivery stability. Stability — the ability to recover quickly when things go wrong — is exactly the metric that accountability protects. When no one owns the change, no one owns the recovery.
→ [dora.dev/dora-report-2025](https://dora.dev/dora-report-2025/)

**The moral crumple zone**

Researcher Madeleine Elish coined the term *moral crumple zone* to describe what happens in automated systems when something goes wrong: the nearest human absorbs the blame for a failure they couldn't fully control. In agentic coding, that human is the prompter — the person whose name is on the PR, who may not have written or fully understood a single line of what they're being held responsible for.

---

## What it costs you

- No clear owner when an incident fires at 2am
- Weakened code review norms — if the AI wrote it, reviewing feels optional
- Audit and compliance exposure — especially as the EU AI Act (enforcement August 2026) makes accountability a legal requirement, not just a cultural one
- The social contract of software engineering erodes: "your name on the PR means you stand behind it" stops being true

---

## The fix

### One named human per AI change

Every AI-generated change that ships needs exactly one accountable human attached to it. Not "the team." Not "the agent." One person who reviewed it, understood it, and is prepared to defend it.

This isn't bureaucracy. It's the minimum condition for accountability to mean anything.

### Provenance tagging

Tag commits and PRs to indicate which parts were AI-generated and which were human-authored. This isn't about blame — it's about audit trails. When something breaks six months later, you need to know whether the relevant code was human-reasoned or AI-generated, and who reviewed it.

A simple convention in your commit messages:

```
fix: normalise error response format [AI-assisted, reviewed by @name]
```

### Scope boundaries in your AGENTS.md

The agent should never be able to modify more than you explicitly authorised. Blast radius control is accountability control.

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

### Review for understanding, not just correctness

"LGTM" on a diff you didn't model is not a review. It's a signature on a blank cheque.

The standard should be: can you explain what this code does and why, to someone who wasn't in the room? If not, the review isn't done.

---

## The connection to the other failure modes

Accountability flows directly from Opacity. You can't own what you don't understand. And when accountability is diffuse, Trust miscalibration follows — developers accept AI output more readily when they've implicitly decided that the responsibility for it is shared.

→ Back: [Overview](../overview.md) · Related: [Opacity](./opacity.md)

---

## Sources

- Dacy, L. (2025). Who Owns AI-Generated Code When It Ships? [big-agile.com](https://big-agile.com/blog/who-owns-ai-generated-code-when-it-ships-building-a-chain-of-human-accountability)
- IBM / CIO.com (2025). CIOs Are Being Held Accountable for AI They Don't Fully Control. [cio.com](https://www.cio.com/article/4182288/cios-are-being-held-accountable-for-ai-they-dont-fully-control-ibm-study-finds.html)
- DORA / Google Cloud (2025). 2025 DORA Report. [dora.dev/dora-report-2025](https://dora.dev/dora-report-2025/)
- Elish, M.C. (2019). Moral Crumple Zones: Cautionary Tales in Human-Robot Interaction. *Engaging Science, Technology, and Society*, 5, 40–60.
- Willison, S. (2025). Vibe coding is not an excuse. [simonwillison.net/2025/Mar/6/vibe-coding](https://simonwillison.net/2025/Mar/6/vibe-coding/)
