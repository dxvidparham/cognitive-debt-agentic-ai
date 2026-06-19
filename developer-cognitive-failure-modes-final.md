# Cognitive & Psychological Failure Modes in Agentic AI-Assisted Software Development
## A Synthesized Research Report

**Compiled:** 2026-06-09  
**Sources:** 3 research clusters + evidence base (see source ledger)  
**Scope:** 6 named failure modes across opacity, accountability, trust, context-switching, flow disruption, and skill atrophy. Prompt debt (mode 7) is out of scope for this synthesis.

> **Evidence grading used throughout:**
> - `STRONG` = RCT, large-N controlled study, or multiple corroborating independent accounts
> - `MODERATE` = named expert / industry report / single study with caveats
> - `WEAK` = single anecdote or blog claim
> - `⚠ UNVERIFIED` = could not confirm — do not cite without checking

---

## PART 1 — The Six Failure Modes


### 1. Opacity / Loss of Comprehension `[OPACITY]`

**Definition.** The developer ships code they do not understand at the level they would understand hand-written code. The artifact exists; the mental model does not.

**Why agentic context makes it worse.** Autocomplete tools produced line-level suggestions a developer still typed *through*. Agentic tools generate and edit across multiple files in autonomous loops. The human moves from *author* to *reviewer of diffs* — and review is a weaker comprehension path than authorship.

**Key evidence.**
- `MODERATE/STRONG` — dhung.dev: *"when I let AI do too much, I feel disconnected to the codebase... It works. But I don't generally get how it works."* Posted to r/ExperiencedDevs with hundreds of corroborating replies. <https://dhung.dev/blog/ai-wrote-my-code-i-couldnt-feel-it>
- `MODERATE` — Simon Willison: *"if you're going to put your name to it you need to be confident that you understand how and why it works — ideally to the point that you can explain it to somebody else."* <https://simonwillison.net/2025/Mar/6/vibe-coding/>

**HCI connection.** Classic deskilling + the **generation effect** in reverse: information you generate yourself is retained better than information you merely review. Agentic workflows optimize away the generation step.

**Downstream consequences.** Slower incident response; review theater (LGTM on diffs no one models); architectural drift; onboarding debt.


### 2. The Accountability Gap `[ACCOUNTABILITY]`

**Definition.** When AI-generated code fails in production, responsibility diffuses. The tool can't be accountable; the human who prompted it didn't write it and may not understand it; the reviewer rubber-stamped a diff. Ownership evaporates at exactly the moment it's needed.

**Why agentic context makes it worse.** Multi-step autonomous agents widen the gap between *intent* (the prompt) and *artifact* (the merged code). The more steps the agent takes unattended, the less any single human can claim "I made this decision."

**Key evidence.**
- `MODERATE` — Big Agile / Lance Dacy: *"We didn't lose accountability. It's right there in the room with us... we diffuse[d] it."* Recommends attaching one named human to every AI-generated change that ships. <https://big-agile.com/blog/who-owns-ai-generated-code-when-it-ships-building-a-chain-of-human-accountability>
- `MODERATE` — Simon Willison: *"you have to take accountability for the code you produce."* His distinction: if you reviewed, tested, and understood every line — accountability intact. Vibe coding without that = accountability gap. <https://simonwillison.net/2025/Mar/6/vibe-coding/>

**HCI connection.** **Moral crumple zone** (Madeleine Elish): in automated systems the nearest human absorbs blame for failures they couldn't fully control. Agentic coding risks making the prompter the crumple zone.

**Downstream consequences.** No clear owner for incidents; weakened code review norms; audit/compliance exposure; erosion of the social contract that "your name on the PR means you stand behind it."


### 3. Trust Miscalibration `[TRUST]`

**Definition.** The developer's trust in the AI's output is not aligned with its actual reliability — either **over-trust** (accepting wrong output, automation bias/complacency) or **under-trust** (re-checking everything, losing the benefit). The damaging direction in practice is over-trust.

**Why agentic context makes it worse.** Agents are fluent and confident regardless of correctness. Fluency is a poor proxy for accuracy, but humans use it as one. Autonomous loops also reduce the number of natural checkpoints where a human would otherwise catch an error.

**Key evidence.**
- `STRONG` — **METR RCT (2025):** 16 experienced OSS developers, 246 real issues. AI made them **~19% slower** while they *believed* they were 20% faster. Developers forecast 24% speedup beforehand; still estimated 20% speedup afterward despite being measurably slower. This perception–reality gap is the cleanest available evidence of trust miscalibration. <https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/>
- `MODERATE` — **Microsoft Research:** 17-developer qualitative study identifying three trust challenges: building appropriate expectations, configuring the tool, and validating suggestions. <https://www.microsoft.com/en-us/research/publication/investigating-and-designing-for-trust-in-ai-powered-code-generation-tools/>
- `STRONG` — **Stanford CCS '23 (Perry et al.):** 47 participants, 5 security tasks. AI users wrote significantly less secure code on 4/5 tasks yet were *more likely to believe their code was secure*. Those who trusted the AI less and engineered prompts more carefully wrote more secure code. <https://arxiv.org/abs/2211.03622>
- `STRONG` (survey) — **Stack Overflow 2025:** Only 3% "highly trust" AI output accuracy; 46% distrust it. Accuracy trust fell from 40% to 29% YoY while adoption rose from 76% to 84% — the adoption–trust gap. <https://survey.stackoverflow.co/2025>

**HCI connection.** Parasuraman & Manzey (2010) — automation complacency and automation bias affect both novices and experts and **cannot be prevented by training or instructions alone**. Lee & See — the goal is *appropriate* reliance, not maximal trust.

**Downstream consequences.** Accepting subtly wrong code; under-testing because "the agent seems confident"; the productivity illusion (teams adopt tools that feel faster but aren't); compounding errors inside unattended agent loops.


### 4. Context-Switching Burden `[CONTEXT]`

**Definition.** Agentic AI workflows impose a new class of context switch: the developer must repeatedly shift between *authoring* (writing code, holding a mental model) and *supervising* (reviewing agent diffs, validating outputs, re-orienting to what the agent changed). Each switch carries a cognitive re-entry cost.

**Why agentic context makes it worse.** Autocomplete-era tools inserted suggestions inline — the developer stayed in the editor, in the file, in the flow. Agentic tools operate across multiple files, spawn terminal commands, and return large diffs. The human must context-switch to a diff view, parse what changed across N files, decide whether to accept, then re-enter the original mental model.

**Key evidence.**
- `STRONG` (survey) — **Stack Overflow 2025:** 45% cite "AI solutions that are almost right, but not quite" as their top frustration; 66% report spending more time fixing "almost-right" AI code. This is direct evidence of a verification-and-correction loop that imposes repeated context switches. <https://survey.stackoverflow.co/2025>
- `MODERATE` — **NSF/PAR 10677745 "The Fast and Spurious" (Afroz et al., 2025):** 415 practitioners, SPACE framework. Faster task completion offset by increased code review burden and persistent cognitive load from output verification. "Perceived productivity gains may be spurious." ⚠ verify citation.
- `MODERATE` — **METR RCT (2025):** Developers spent ~9% of total task time reviewing and cleaning up AI output; 56% reported major edits required. This quantifies the supervision overhead as a structural context-switch tax.

**HCI connection.** Classic task-switching cost research (Monsell, 2003) shows switching between tasks of different cognitive types incurs a switch cost of 200–400ms per switch, compounding under high-frequency switching. Agentic workflows structurally increase switch frequency.

**Downstream consequences.** Fragmented mental models; review fatigue; slower deep work; supervision overhead erasing the raw generation speedup.


### 5. Flow-State Disruption `[FLOW]`

**Definition.** Agentic AI tools interrupt the psychological state of *flow* — Csikszentmihalyi's condition of deep, effortful, intrinsically rewarding engagement with a challenging task. Flow requires: clear goals, immediate feedback, skill-challenge balance, and uninterrupted concentration. Agentic tools alter all four conditions.

**Why agentic context makes it worse.** Flow requires the developer to be the agent of the work — to struggle, to make micro-decisions, to hold the whole problem in mind. Delegating to an AI agent removes the struggle and the micro-decisions. The developer becomes a supervisor rather than a craftsperson.

**Key evidence.**
- `MODERATE` — **HN "Lost the Joy of Programming" thread (2025):** High-engagement thread where experienced developers describe losing intrinsic motivation and flow when using agentic tools. Representative quotes describe the shift from "I built this" to "I supervised this" as psychologically hollow. Hundreds of comments, high upvotes — a documented community-level phenomenon. <https://news.ycombinator.com/item?id=44499063>
- `MODERATE` — **METR RCT (2025):** Developers *felt* more productive (the subjective experience of AI-assisted work is engaging) but were measurably slower. The AI-assisted workflow produces a *feeling* of flow without the actual deep engagement that produces quality work.
- `MODERATE` — **DORA 2025:** AI adoption positively associated with individual-level flow and job satisfaction — the counter-evidence. For some developers, AI tools *enhance* flow by removing tedious boilerplate. The failure mode is not universal.
- `MODERATE` — **Stack Overflow 2025:** 66% spending more time fixing "almost-right" code. The "almost-right" problem is a flow-killer: the developer enters a debugging/verification loop on code they didn't write.

**Psychology connection.** Csikszentmihalyi (1990) — flow requires autotelic engagement: the activity is its own reward. Supervision is not autotelic. Self-Determination Theory (Deci & Ryan) — intrinsic motivation requires autonomy, competence, and relatedness. Agentic delegation reduces perceived autonomy and competence-demonstration.

**Downstream consequences.** Reduced intrinsic motivation; developer burnout from supervision-heavy work; loss of the "joy of programming" as a retention factor; degraded code quality when developers disengage from the supervisory role.


### 6. Skill Atrophy / Deskilling `[ATROPHY]`

**Definition.** Sustained delegation of cognitive work to AI agents degrades the developer's ability to perform that work independently. The skills that atrophy first are those most frequently delegated: debugging, algorithm design, API recall, reading unfamiliar code, and writing from scratch.

**Why agentic context makes it worse.** Skill maintenance requires practice under challenge. Agentic tools remove challenge at exactly the points where practice would occur. A developer who never debugs without AI assistance loses the debugging mental models that come from repeated independent struggle. This is Bainbridge's "ironies of automation" applied to software: the human is kept in the loop for oversight but denied the practice that would make their oversight effective.

**Key evidence.**
- `STRONG` — **MSR+CMU Lee et al. (CHI 2025):** 319 knowledge workers, 936 task examples. Higher confidence in GenAI → less critical thinking (β = −0.69, p < 0.001). Majority report reduced effort across all six Bloom's categories. GenAI shifts work from execution to oversight — the proximate mechanism of skill atrophy. <https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/>
- `MODERATE` — **openreview.net WpL9rAd0K7 (RCT, 2025):** AI use impairs code reading, debugging, and conceptual understanding of libraries. Full delegation showed some productivity gains "at the cost of learning the library." Six AI interaction patterns identified; the most delegating patterns showed the steepest comprehension loss. ⚠ verify venue before citing as peer-reviewed.
- `STRONG` (foundational) — **Parasuraman & Manzey (2010):** Automation complacency cannot be overcome by practice alone; affects experts and novices equally; cannot be prevented by training or instructions. If the mechanism is structural, skill atrophy from AI delegation is not self-correcting. <https://journals.sagepub.com/doi/10.1177/0018720810376055>

**The longitudinal gap.** No published long-term study tracks measured decline in developer skill from sustained AI use. All "deskilling" claims are cross-sectional or theoretical. This is the single biggest evidentiary gap in the entire failure-mode literature.

**HCI connection.** Bainbridge (1983) "Ironies of Automation" — the human kept in the loop for oversight is denied the practice that would make their oversight competent. Cognitive load theory (Sweller) — skills are built through effortful processing; removing effort removes the learning signal.

**Downstream consequences.** Developers who cannot work without AI assistance (cognitive vendor lock-in); degraded ability to review AI output (the oversight loop fails because the overseer's skills have atrophied); junior developers who never build foundational skills; organizational fragility when AI tools are unavailable or wrong.


---

## PART 2 — Guardrails (by layer)

### Individual habits

| Habit | Modes addressed |
|-------|----------------|
| **Comprehension gate.** Before merging AI code, be able to explain *how and why it works* to another person (Willison's bar). If you can't, you don't merge. | OPACITY, ACCOUNTABILITY |
| **Author the hard parts.** Hand-write security-, money-, and concurrency-critical paths to preserve the generation effect where it matters most. | OPACITY, ATROPHY |
| **Deliberate practice windows.** Reserve blocks of time for AI-free coding on non-critical tasks — the equivalent of a surgeon practicing sutures. | ATROPHY |
| **Distrust fluency.** Treat confident output as *unverified by default*; calibrate trust to demonstrated reliability on *this* codebase, not the tool's tone. | TRUST |
| **Measure, don't feel.** METR shows perceived speedup is unreliable — track actual cycle time / defect rates rather than trusting the feeling of speed. | TRUST |
| **Batch supervision.** Rather than context-switching on every agent suggestion, batch reviews to dedicated review windows — reduces switch frequency. | CONTEXT |
| **Flow protection.** Use AI for setup/scaffolding *before* entering deep work; avoid invoking agents mid-flow. | FLOW |

### Team conventions

| Convention | Modes addressed |
|-----------|----------------|
| **One named human per AI change.** Every AI-generated change that ships has exactly one accountable human attached (Big Agile). | ACCOUNTABILITY |
| **Review-for-understanding norm.** Reviewers must model the change, not just diff it; "I couldn't follow this" is a valid block. | OPACITY, ACCOUNTABILITY, ATROPHY |
| **Pairing on agent output.** Keep a second human in the loop for non-trivial agent work — distributes supervision cost and maintains two people's mental models. | OPACITY, CONTEXT, ATROPHY |
| **Skill audit in retrospectives.** Periodically ask: "What can we no longer do without AI?" Flag atrophied skills before they become critical gaps. | ATROPHY |

### Tooling

| Tooling change | Modes addressed |
|---------------|----------------|
| **Provenance metadata.** Tag commits/PRs with which parts were AI-generated and which human-authored, so accountability and audit trails survive. | ACCOUNTABILITY |
| **Friction at delegation boundaries.** Require explicit human approval before agent proceeds to next file/module — reduces the diff-size that triggers the worst context-switch costs. | CONTEXT, TRUST |
| **Explicit context files (CLAUDE.md / AGENTS.md pattern).** Writing constraints and goals explicitly restores authorial engagement and reduces the "almost-right" correction loop. | FLOW, CONTEXT |
| **Validation surfaces.** Make it easy to validate suggestions — tests, diffs, explanations surfaced inline. | TRUST |


---

## PART 3 — Cross-Cutting Patterns

### 1. The six modes form a causal chain, not six separate problems

**Opacity → Accountability → Trust → Context → Flow → Atrophy**

- You can't be accountable for what you don't understand (Opacity → Accountability)
- You over-trust the tool to compensate for what you can't verify (Accountability → Trust)
- Over-trust means accepting large diffs you don't model, increasing supervision burden (Trust → Context)
- High supervision burden fragments deep work and kills flow (Context → Flow)
- Degraded flow means less effortful practice, accelerating skill atrophy (Flow → Atrophy)
- Atrophied skills make comprehension harder, closing the loop (Atrophy → Opacity)

Mitigating *comprehension* (Mode 1) is upstream of all other modes.

### 2. Perception is systematically unreliable — in both directions

- METR (field RCT): devs *over*-estimated their gain (felt 20% faster, were 19% slower)
- GitHub/Peng (lab RCT): devs *under*-estimated their gain (felt 35% faster, were 55.8% faster)

Common thread: developer self-perception of AI productivity is **unreliable in either direction**. This undermines the self-report basis of every survey-based productivity claim (DORA, Stack Overflow). Guardrails must be *external and measured*, not introspective.

### 3. The agentic shift is author → reviewer

All six modes trace to the same structural root: the human's role moved from *generating* code to *reviewing* it. Review is a weaker channel for comprehension, ownership, calibration, flow, and skill-building alike. The most effective mitigations restore authorial engagement — hand-writing the hard parts, pairing, comprehension gates.

### 4. The strongest fixes are social, not technical

"One named human," "explain it to someone else," "keep pairing," "skill audits in retros" — the most-cited mitigations restore *human accountability structures* that the tooling eroded. Technical fixes (provenance tags, approval gates) are necessary but insufficient without the social layer.

### 5. Flow evidence is genuinely mixed — resist a single narrative

DORA 2025 shows AI *increases* individual flow for some developers. The HN thread shows the opposite. These may measure different populations (those who value output vs. those who value craft) or different task types (boilerplate vs. core logic). The failure mode is real but not universal.

### 6. The productivity contradiction is reconcilable

- AI helps most where the human has *least* context (juniors, greenfield, boilerplate) — GitHub/MIT RCTs
- AI can *hurt* where the human already holds deep implicit context the model lacks (experts, large legacy repos) — METR RCT

These are not in conflict. The danger is generalizing either result.


---

## PART 4 — Open Questions & Thin Evidence

### Critical gaps (highest priority)

| Gap | Why it matters |
|-----|---------------|
| **No longitudinal skill-atrophy study exists.** All deskilling claims are cross-sectional or mechanistic inference. | The concern is well-motivated by mechanism but not demonstrated. This is the single biggest gap in the literature. |
| **Agentic (autonomous multi-step) workflows are almost unmeasured.** Almost all rigorous evidence concerns autocomplete/chat assistants, not fully agentic coding agents. | The agentic-specific cognitive load — prompt engineering, reviewing large autonomous diffs, accountability for code the dev never wrote — is currently inference from A1's review-burden data + B1's oversight-shift finding. |
| **Opacity has no hard outcome study.** The strongest opacity evidence is qualitative (essay + thread + Willison). | No study yet links AI-induced opacity to measurable incident-response or defect outcomes. Strong signal, weak measurement. |
| **Flow disruption is anecdotal.** No controlled study of flow-state frequency/depth in AI-assisted vs. unassisted coding was found. | Context-switching cost is inferred, not measured. |

### Contradictions to resolve in downstream use

1. **Productivity up vs. down.** Lab RCTs (+55.8%, +26%) vs. METR field RCT (−19%). Reconcileable by population/task type — but requires care when citing.
2. **DORA 2024 vs. DORA 2025 throughput reversal.** −1.5% → positive. DORA attributes to organizational learning but notes the metric was "measured differently." Cite v.2024.3 for 2024 figures.
3. **Trust trend direction.** Stack Overflow: accuracy-trust falling (40%→29%). DORA: distrust easing slightly (39%→30%). Both agree trust is low and adoption is near-universal.
4. **Flow up vs. flow lost.** DORA 2025 (individual flow increases) vs. HN thread (joy of programming lost). Different populations, different task types, or different definitions of flow.

### Items flagged as unverified — do not cite without checking

- **arXiv 2604.18948** — future-dated (2026), suspicious ID. Do not cite.
- **openreview.net WpL9rAd0K7** — RCT on comprehension loss. Verify venue/acceptance status before citing as peer-reviewed.
- **NSF/PAR 10677745 "The Fast and Spurious"** — verify full author list, venue, and exact figures against NSF PAR database.
- **Bainbridge (1983) "Ironies of Automation"** and **Parasuraman & Riley (1997)** — canonical and real, but not independently opened this pass. Treat exact page/figure claims as unconfirmed until pulled directly.
- **DORA 2024 burnout figure (−9.9%)** — from errata page correcting Figure 18. Confirm against corrected PDF (v.2024.3) before quoting.


---

## Source Ledger

### Tier A — Strongest causal / large-N evidence

| ID | Source | Modes | Grade |
|----|--------|-------|-------|
| A1 | METR RCT (2025) — 16 devs, 246 tasks, −19% slower, perception gap | TRUST, CONTEXT, FLOW, ATROPHY | STRONG |
| A2 | Peng/GitHub RCT (2023) — 95 devs, +55.8% faster (lab task) | TRUST (counter) | STRONG |
| A3 | MIT 3-firm RCT (2024) — 4,867 devs, +26% tasks completed | TRUST (counter), ATROPHY | STRONG |

### Tier B — Solid peer-reviewed controlled studies

| ID | Source | Modes | Grade |
|----|--------|-------|-------|
| B1 | MSR+CMU Lee et al. CHI 2025 — 319 workers, critical thinking reduction (β=−0.69) | ATROPHY, TRUST | STRONG |
| B2 | Stanford CCS 2023 — 47 devs, less secure code + overconfidence on 4/5 tasks | TRUST | STRONG |
| B3 | openreview WpL9rAd0K7 — comprehension loss RCT (2025) | ATROPHY, OPACITY | MODERATE ⚠ verify venue |

### Tier C — Large-N industry surveys (directional, mostly self-report)

| ID | Source | Modes | Grade |
|----|--------|-------|-------|
| C1 | DORA 2024 — >39,000 professionals, −1.5% throughput, 39% distrust | TRUST, ACCOUNTABILITY | MODERATE |
| C2 | DORA 2025 — ~5,000 professionals, throughput now +, stability still −, 30% distrust | TRUST, FLOW | MODERATE |
| C3 | Stack Overflow 2025 — 49,000+ devs, 46% distrust, 66% fixing almost-right code | TRUST, CONTEXT, FLOW | STRONG (survey) |
| C4 | NSF/PAR 10677745 "The Fast and Spurious" — 415 practitioners, review burden | CONTEXT, ATROPHY | MODERATE ⚠ verify |

### Tier D — Foundational cognitive-science / HCI mechanisms

| ID | Source | Modes | Grade |
|----|--------|-------|-------|
| D1 | Parasuraman & Manzey (2010) — complacency/bias; experts not immune; training doesn't fix | TRUST, ATROPHY | STRONG (foundational) |
| D2 | Bainbridge (1983) — Ironies of Automation | ATROPHY, CONTEXT | STRONG (foundational) ⚠ not independently verified |
| D3 | Parasuraman & Riley (1997) — misuse/disuse taxonomy | ATROPHY | STRONG (foundational) ⚠ not independently verified |

### Practitioner / qualitative sources

| ID | Source | Modes | Grade |
|----|--------|-------|-------|
| P1 | dhung.dev — "AI Wrote My Code. I Couldn't FEEL It." | OPACITY | MODERATE/STRONG |
| P2 | Simon Willison — vibe-coding (2025-03-06) | OPACITY, ACCOUNTABILITY, ATROPHY | MODERATE |
| P3 | Big Agile / Lance Dacy — "Who Owns AI-Generated Code…" | ACCOUNTABILITY | MODERATE |
| P4 | Thoughtworks / Böckeler — *Exploring Gen AI* memos | TRUST, OPACITY | MODERATE |
| P5 | HN "Lost the Joy of Programming" thread (2025) | FLOW | MODERATE (community signal) |
| P6 | arXiv 2507.08149 — coding agent controlled study (2025) | CONTEXT | MODERATE ⚠ verify |

---

## Appendix — Source Files

This synthesis draws from three research cluster files:

- `cognitive-failure-modes-agentic-ai.md` — Modes 1–3 (opacity, accountability, trust) with full evidence and guardrails
- `flow-contextswitch-skillatrophy-modes.md` — Modes 4–6 (context-switching, flow, skill atrophy) with full evidence and guardrails
- `ai-dev-cognitive-effects-evidence-base.md` — Full evidence base with reliability tiers, exact figures, caveats, and contradiction analysis

