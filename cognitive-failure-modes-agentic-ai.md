# Three Cognitive Failure Modes in Agentic AI Coding

> **Scope note.** This report covers exactly three failure modes — **(1) Opacity / loss of comprehension**, **(2) The accountability gap**, and **(3) Trust miscalibration**. Other modes (context switching, skill atrophy, flow disruption, prompt debt) are deliberately out of scope and owned by other agents. Findings are tagged `[OPACITY]`, `[ACCOUNTABILITY]`, `[TRUST]` for downstream synthesis.
>
> **Evidence grading.** `STRONG` = controlled study or multiple corroborating independent accounts. `MODERATE` = named expert / industry report / single study with caveats. `WEAK` = single anecdote or blog claim. Anything I could not verify is marked `⚠ UNVERIFIED` and should not be cited downstream without checking.

---

## PART 1 — The Catalogue

### 1. Opacity / Loss of Comprehension `[OPACITY]`

**Definition.** The developer ships, owns, or is responsible for code they do not understand at the level they would understand hand-written code. The artifact exists; the mental model does not.

**Why agentic context makes it worse.** Earlier autocomplete (Copilot-style) produced line-level suggestions a developer still typed *through*. Agentic tools (Cursor agent mode, Claude Code, etc.) generate, edit across multiple files, and self-correct in loops. The human moves from *author* to *reviewer of diffs*, and review is a weaker comprehension path than authorship.

**Symptoms & evidence.**

- `MODERATE/STRONG` — Practitioner essay corroborated by a large thread. dhung.dev, *"AI Wrote My Code. I Couldn't FEEL It."*
  <https://dhung.dev/blog/ai-wrote-my-code-i-couldnt-feel-it>
  > "when I let AI do too much, I feel disconnected to the codebase... It works. But I don't generally get how it works."
  > "reviewing is not the same as understanding... The intention gap is real."
  Posted to r/ExperiencedDevs with hundreds of corroborating replies — the *volume of agreement* is what raises this from WEAK to MODERATE/STRONG.

- `MODERATE` — Simon Willison frames comprehension as the dividing line, not a nice-to-have. <https://simonwillison.net/2025/Mar/6/vibe-coding/>
  > "if you're going to put your name to it you need to be confident that you understand how and why it works — ideally to the point that you can explain it to somebody else."

**HCI connection.** This is classic **deskilling** + the **generation effect** in reverse: information you generate yourself is retained better than information you merely review. Agentic workflows optimize away the generation step.

**Downstream consequences.** Slower incident response (you can't debug what you don't understand); review theater (LGTM on diffs no one models); architectural drift (no one holds the whole picture); onboarding debt (the "author" can't explain the system).

---

### 2. The Accountability Gap `[ACCOUNTABILITY]`

**Definition.** When AI-generated code fails in production, responsibility diffuses. The tool can't be accountable; the human who prompted it didn't write it and may not understand it; the reviewer rubber-stamped a diff. Ownership evaporates at exactly the moment it's needed.

**Why agentic context makes it worse.** Multi-step autonomous agents widen the gap between *intent* (the prompt) and *artifact* (the merged code). The more steps the agent takes unattended, the less any single human can claim "I made this decision."

**Symptoms & evidence.**

- `MODERATE` — Big Agile / Lance Dacy, *"Who Owns AI-Generated Code When It Ships to Production?"*
  <https://big-agile.com/blog/who-owns-ai-generated-code-when-it-ships-building-a-chain-of-human-accountability>
  > "We didn't lose accountability. It's right there in the room with us... we diffuse[d] it."
  Includes a post-mortem dialogue ("I prompted Copilot" / "it compiled" / "functionality looked good") and recommends **attaching one named human to every AI-generated change that ships**. Practitioner argument, not a study — graded MODERATE for being a named, reasoned source rather than corroborated data.

- `MODERATE` — Simon Willison ties accountability directly to comprehension. <https://simonwillison.net/2025/Mar/6/vibe-coding/>
  > "you have to take accountability for the code you produce."
  His distinction: if you reviewed, tested, and understood every line, "that's using an LLM as a typing assistant" — accountability intact. Vibe coding without that = accountability gap.

**HCI connection.** **Moral crumple zone** (Madeleine Elish): in automated systems the nearest human absorbs blame for failures they couldn't fully control. Agentic coding risks making the prompter the crumple zone.

**Downstream consequences.** No clear owner for incidents; weakened code review norms; audit/compliance exposure (who attests to this code?); erosion of the social contract that "your name on the PR means you stand behind it."

---

### 3. Trust Miscalibration `[TRUST]`

**Definition.** The developer's trust in the AI's output is not aligned with its actual reliability — either **over-trust** (accepting wrong output, automation bias/complacency) or **under-trust** (re-checking everything, losing the benefit). The damaging direction in practice is over-trust.

**Why agentic context makes it worse.** Agents are fluent and confident regardless of correctness. Fluency is a poor proxy for accuracy, but humans use it as one. Autonomous loops also reduce the number of natural "checkpoints" where a human would otherwise catch an error.

**Symptoms & evidence.**

- `STRONG` — **METR randomized controlled trial.** *"Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity."*
  <https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/>
  16 experienced open-source developers, 246 real issues, RCT design (Cursor Pro + Claude 3.5/3.7 Sonnet). Result: AI made them **~19% slower** — *while they believed they were faster.* This perception–reality gap is the cleanest available evidence of trust miscalibration: subjective trust diverged from objective performance. **This is the single strongest citation in the report.**

- `MODERATE` — Microsoft Research, *"Investigating and Designing for Trust in AI-powered Code Generation Tools."*
  <https://www.microsoft.com/en-us/research/publication/investigating-and-designing-for-trust-in-ai-powered-code-generation-tools/>
  17-developer qualitative study identifying three trust challenges: **building appropriate expectations, configuring the tool, and validating suggestions.** Names the calibration problem directly.

- `MODERATE` — Thoughtworks / Martin Fowler, *Exploring Gen AI* memos (Birgitta Böckeler).
  <https://martinfowler.com/articles/exploring-gen-ai.html>
  Relevant memos: "How to tackle the unreliability of coding assistants" and "Coding assistants do not replace pair programming." Treats unreliability as a property to *manage*, implying calibration discipline.

- `⚠ UNVERIFIED` — arXiv: *"Relationships Between Trust, Compliance, and Performance for Novice Programmers Using AI Code Generation"* (ID 2604.18948, listed 2026). **Do not cite downstream without verifying the ID and date** — the future-dated ID is suspicious. Flagged for the synthesis owner.

**HCI connection.** Directly maps to the foundational automation literature:
- **Parasuraman & Manzey** — *automation complacency* and *automation bias* (omission + commission errors).
- **Lee & See**, *"Trust in Automation: Designing for Appropriate Reliance"* — the canonical statement that the goal is *appropriate* reliance, not maximal trust. METR's finding is a live instance of *inappropriate* reliance.

**Downstream consequences.** Accepting subtly wrong code; under-testing because "the agent seems confident"; the productivity *illusion* (teams adopt tools that feel faster but aren't); compounding errors inside unattended agent loops.

---

## PART 2 — Guardrails (separated by layer)

### Individual habits
- **Comprehension gate.** Before merging AI code, be able to explain *how and why it works to another person* (Willison's bar). If you can't, you don't merge. `[OPACITY][ACCOUNTABILITY]`
- **Author the hard parts.** Hand-write the security-, money-, and concurrency-critical paths to preserve the generation effect where it matters most. `[OPACITY]`
- **Distrust fluency.** Treat confident output as *unverified by default*; calibrate trust to demonstrated reliability on *this* codebase, not the tool's tone. `[TRUST]`
- **Measure, don't feel.** METR shows perceived speedup is unreliable — track actual cycle time / defect rates rather than trusting the feeling of speed. `[TRUST]`

### Team conventions
- **One named human per AI change.** Every AI-generated change that ships has exactly one accountable human attached (Big Agile). `[ACCOUNTABILITY]`
- **Review-for-understanding norm.** Reviewers must model the change, not just diff it; "I couldn't follow this" is a valid block. `[OPACITY][ACCOUNTABILITY]`
- **Pairing on agent output.** Thoughtworks' point that assistants *don't replace* pairing — keep a second human in the loop for non-trivial agent work. `[OPACITY][TRUST]`

### Tooling
- **Provenance metadata.** Tag commits/PRs with which parts were AI-generated and which human-authored, so accountability and audit trails survive. `[ACCOUNTABILITY]`
- **Friction at the right checkpoints.** Configure agents to require human approval at high-risk boundaries (schema/migrations, auth, payments) rather than running fully unattended. `[TRUST]`
- **Validation surfaces.** Microsoft Research's third challenge — make it *easy to validate* suggestions (tests, diffs, explanations surfaced inline). `[TRUST]`

---

## PART 3 — Patterns Across the Three Modes

1. **The three modes form a causal chain, not three separate problems.** Opacity → you can't be accountable for what you don't understand → so you over-trust the tool to compensate. Mitigating *comprehension* is upstream of both other modes.
2. **Perception is systematically unreliable.** dhung.dev "feels disconnected," METR devs "feel faster" but are slower. Subjective signals (feeling productive, feeling confident) are exactly the signals these failure modes corrupt — so guardrails must be *external and measured*, not introspective.
3. **The agentic shift is author → reviewer.** All three modes trace to the same root: the human's role moved from generating code to reviewing it, and review is a weaker channel for comprehension, ownership, and calibration alike.
4. **The strongest fixes are social, not technical.** "One named human," "explain it to someone else," "keep pairing" — the most cited mitigations restore *human accountability structures* the tooling eroded.

---

## PART 4 — Open Questions & Thin Evidence

- **Thin: the accountability gap rests on practitioner argument, not data.** The Big Agile source is reasoned and named but is not a study. No controlled or large-N evidence quantifies how ownership actually diffuses with AI code. **Gap worth flagging to synthesis.**
- **Verify before use:** arXiv `2604.18948` (novice trust/compliance) is future-dated (2026) and unverified. Do not cite without confirming the identifier.
- **Trust evidence skews to experts.** METR and Microsoft Research both study experienced/professional developers. Whether juniors miscalibrate *worse* (more over-trust, less ability to validate) is plausible but **not established here** — the one source that would address it is the unverified arXiv paper.
- **Opacity has no hard outcome study.** The strongest opacity evidence is qualitative (essay + thread + Willison). No study yet links AI-induced opacity to measurable incident-response or defect outcomes. Strong *signal*, weak *measurement*.
- **Calibration over time is unknown.** Does trust calibrate toward accuracy with experience using a given agent, or does complacency deepen? No longitudinal evidence found.
- **Definitional overlap to resolve in synthesis.** Opacity and trust miscalibration interact (you over-trust *because* you can't verify). Synthesis should decide whether to treat the comprehension→trust link as one mechanism or two.

---

### Source ledger (verifiable)
| # | Source | Mode | Grade |
|---|--------|------|-------|
| 1 | dhung.dev — "AI Wrote My Code. I Couldn't FEEL It." | OPACITY | MOD/STRONG |
| 2 | Simon Willison — vibe-coding (2025-03-06) | OPACITY, ACCOUNTABILITY | MODERATE |
| 3 | Big Agile / Lance Dacy — "Who Owns AI-Generated Code…" | ACCOUNTABILITY | MODERATE |
| 4 | METR RCT (2025-07-10) — 19% slower / perception gap | TRUST | STRONG |
| 5 | Microsoft Research — "…Trust in AI-powered Code Generation Tools" | TRUST | MODERATE |
| 6 | Thoughtworks / Fowler — *Exploring Gen AI* (Böckeler) | TRUST, OPACITY | MODERATE |
| 7 | Lee & See; Parasuraman & Manzey (HCI automation literature) | TRUST | STRONG (foundational) |
| 8 | arXiv 2604.18948 — novice trust/compliance | TRUST | ⚠ UNVERIFIED |
