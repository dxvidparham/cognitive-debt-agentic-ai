# Three Cognitive Failure Modes in Agentic AI Coding — Cluster B
## Context-Switching Burden, Flow-State Disruption, Skill Atrophy / Deskilling

> **Scope note.** This report covers **(4) Context-switching burden**, **(5) Flow-state disruption**, and **(6) Skill atrophy / deskilling**. Findings are tagged `[CONTEXT]`, `[FLOW]`, `[ATROPHY]` for downstream synthesis. Mode 7 (Prompt debt) is covered separately.
>
> **Evidence grading.** `STRONG` = controlled study or multiple corroborating independent accounts. `MODERATE` = named expert / industry report / single study with caveats. `WEAK` = single anecdote or blog claim. Anything unverified is marked `⚠ UNVERIFIED`.

---

## PART 1 — The Catalogue

### 4. Context-Switching Burden `[CONTEXT]`

**Definition.** Agentic AI workflows impose a new class of context switch: the developer must repeatedly shift between *authoring* (writing code, holding a mental model) and *supervising* (reviewing agent diffs, validating outputs, re-orienting to what the agent changed). Each switch carries a cognitive re-entry cost.

**Why agentic context makes it worse.** Autocomplete-era tools inserted suggestions inline — the developer stayed in the editor, in the file, in the flow. Agentic tools operate across multiple files, spawn terminal commands, and return large diffs. The human must context-switch to a diff view, parse what changed across N files, decide whether to accept, then re-enter the original mental model. The supervision loop is qualitatively different from line-by-line suggestion acceptance.

**Symptoms & evidence.**

- `STRONG` — **Stack Overflow Developer Survey 2025** (>49,000 devs).
  <https://survey.stackoverflow.co/2025>
  **45%** of respondents cite "AI solutions that are almost right, but not quite" as their top frustration; **66%** report spending more time fixing "almost-right" AI code. This is direct evidence of a verification-and-correction loop that imposes repeated context switches: write → delegate → review → fix → re-orient.

- `MODERATE` — **NSF/PAR 10677745 — "The Fast and Spurious" (Afroz et al., 2025-04-26).**
  <https://par.nsf.gov/biblio/10677745>
  Survey of **415 practitioners** using the SPACE framework. Key finding: faster task completion was offset by **increased code review burden** and **persistent cognitive load from output verification**. Authors conclude "perceived productivity gains may be spurious" when verification overhead is accounted for. The review burden is a structural context-switch cost: every AI-generated chunk requires a context switch into reviewer mode.

- `MODERATE` — **arXiv 2507.08149 — First controlled study of developer interactions with coding agents** (2025).
  <https://arxiv.org/abs/2507.08149>
  Controlled comparison of copilot-style vs. agent-style interactions. Agents produced **35% higher task correctness** and **50% less user effort** on the primary task — but the study explicitly flags that "challenges remain ensuring users adequately understand agent behaviors." The supervision gap is the context-switching cost: users must shift from task execution to agent-behavior monitoring.

- `MODERATE` — **METR RCT (2025)** — reviewed AI-generated code consumed ~9% of total task time.
  <https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/>
  Developers spent ~9% of their time reviewing and cleaning up AI output — a context-switch tax paid on every agent invocation. Combined with the 56% who reported major edits required, this quantifies the supervision overhead.

**HCI connection.** Classic **task-switching cost** research (Monsell, 2003; Rubinstein, Meyer & Evans, 2001) shows that switching between tasks of different cognitive types (authoring vs. reviewing) incurs a "switch cost" of 200–400ms per switch, compounding under high-frequency switching. Agentic workflows structurally increase switch frequency.

**Downstream consequences.** Fragmented mental models (the developer never holds the full picture at once); review fatigue (the 66% "almost-right" fixing burden); slower deep work; the supervision overhead erasing the raw generation speedup.

---

### 5. Flow-State Disruption `[FLOW]`

**Definition.** Agentic AI tools interrupt the psychological state of *flow* — Csikszentmihalyi's condition of deep, effortful, intrinsically rewarding engagement with a challenging task. Flow requires: clear goals, immediate feedback, skill-challenge balance, and uninterrupted concentration. Agentic tools alter all four conditions.

**Why agentic context makes it worse.** Flow requires the developer to be the agent of the work — to struggle, to make micro-decisions, to hold the whole problem in mind. Delegating to an AI agent removes the struggle and the micro-decisions. The developer becomes a supervisor rather than a craftsperson. Even when the output is correct, the psychological engagement is qualitatively different — and for many developers, less rewarding.

**Symptoms & evidence.**

- `MODERATE` — **Hacker News: "Lost the Joy of Programming" thread** (2025).
  <https://news.ycombinator.com/item?id=44499063>
  High-engagement HN thread where experienced developers describe losing intrinsic motivation and flow when using agentic tools. Representative quotes describe the shift from "I built this" to "I supervised this" as psychologically hollow. The thread's engagement (hundreds of comments, high upvotes) elevates this from a single anecdote to a documented community-level phenomenon. `WEAK` as individual anecdote; `MODERATE` as community signal.

- `MODERATE` — **METR RCT (2025)** — developers reported AI-assisted work felt faster despite being slower.
  <https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/>
  The perception-reality gap in METR is partly a flow-disruption signal: developers *felt* more productive (the subjective experience of AI-assisted work is engaging) but were measurably slower. This suggests the AI-assisted workflow produces a *feeling* of flow (rapid output, forward motion) without the actual deep engagement that produces quality work.

- `MODERATE` — **DORA 2025** — AI adoption associated with increased individual flow and job satisfaction, but delivery stability still negative.
  <https://dora.dev/dora-report-2025/>
  DORA 2025 reports that AI adoption is positively associated with individual-level flow and job satisfaction. This is the counter-evidence: for some developers, AI tools *enhance* flow by removing tedious boilerplate. The failure mode is not universal — it depends on whether the developer values the struggle or the output.

- `MODERATE` — **Stack Overflow 2025** — 66% spending more time fixing "almost-right" code.
  <https://survey.stackoverflow.co/2025>
  The "almost-right" problem is a flow-killer: the developer enters a debugging/verification loop on code they didn't write, for a problem they didn't fully specify. This is the opposite of flow — it is reactive, externally-driven, and cognitively fragmented.

- `WEAK` — **codewithseb / CLAUDE.md pattern** (practitioner blog, 2025).
  Practitioners adopting explicit `CLAUDE.md` / `AGENTS.md` context files report that the discipline of writing these files (specifying constraints, patterns, goals) partially restores the authorial engagement that agentic delegation removes. This is an emergent workaround for flow disruption, not a study.

**HCI / psychology connection.**
- **Csikszentmihalyi (1990)** — flow requires autotelic engagement: the activity is its own reward. Supervision is not autotelic.
- **Self-Determination Theory (Deci & Ryan)** — intrinsic motivation requires autonomy, competence, and relatedness. Agentic delegation reduces perceived autonomy and competence-demonstration.
- **"Generation effect" (Slamecka & Graf, 1978)** — information you generate yourself is retained and valued more than information you receive. Agentic tools systematically reduce generation.

**Downstream consequences.** Reduced intrinsic motivation; developer burnout from supervision-heavy work; loss of the "joy of programming" as a retention and recruitment factor; degraded code quality when developers disengage from the supervisory role.

---

### 6. Skill Atrophy / Deskilling `[ATROPHY]`

**Definition.** Sustained delegation of cognitive work to AI agents degrades the developer's ability to perform that work independently. The skills that atrophy first are those most frequently delegated: debugging, algorithm design, API recall, reading unfamiliar code, and writing from scratch.

**Why agentic context makes it worse.** Skill maintenance requires practice under challenge. Agentic tools remove challenge at exactly the points where practice would occur. A developer who never debugs without AI assistance loses the debugging mental models that come from repeated independent struggle. This is Bainbridge's "ironies of automation" applied to software: the human is kept in the loop for oversight but denied the practice that would make their oversight effective.

**Symptoms & evidence.**

- `STRONG` — **Microsoft Research + CMU (Lee et al., 2025, CHI)** — GenAI reduces critical thinking effort.
  <https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/>
  319 knowledge workers, 936 task examples. **Higher confidence in GenAI → less critical thinking** (β = −0.69, p < 0.001). Majority report reduced effort across all six Bloom's categories. GenAI shifts work from execution to oversight. This is the proximate mechanism of skill atrophy: if you never exert the cognitive effort, you never build or maintain the skill.

- `MODERATE` — **openreview.net WpL9rAd0K7 — RCT on AI use impairing conceptual understanding** (2025).
  <https://openreview.net/forum?id=WpL9rAd0K7>
  RCT showing AI use impairs code reading, debugging, and conceptual understanding of libraries. Full delegation showed some productivity gains "at the cost of learning the library." Six AI interaction patterns identified; the most delegating patterns showed the steepest comprehension loss. This is the closest available controlled evidence of skill atrophy in a coding context.

- `MODERATE` — **METR RCT (2025)** — experienced devs on familiar codebases were slowed by AI.
  <https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/>
  The METR result is consistent with a skill-atrophy hypothesis: experienced developers who have been using AI tools may have partially degraded the deep-codebase intuitions that made them fast. However, METR explicitly does not measure longitudinal skill change — this interpretation is inferential.

- `MODERATE` — **NSF/PAR 10677745 — "The Fast and Spurious" (Afroz et al., 2025).**
  <https://par.nsf.gov/biblio/10677745>
  415 practitioners. Persistent cognitive load from output verification. The verification burden implies that developers are not building the mental models that would make verification fast — they are perpetually re-learning what the AI generated rather than accumulating durable skill.

- `MODERATE` — **Parasuraman & Manzey (2010)** — automation complacency cannot be overcome by practice alone.
  <https://journals.sagepub.com/doi/10.1177/0018720810376055>
  The canonical automation-complacency review. Key finding for skill atrophy: complacency and bias affect both novices and experts, and **cannot be prevented by training or instructions alone**. If the mechanism is structural (attention allocation under multi-task load), skill atrophy from AI delegation is not self-correcting.

- `WEAK` — **Simon Willison (2025)** — "vibe coding" as a skill-atrophy accelerant.
  <https://simonwillison.net/2025/Mar/6/vibe-coding/>
  Willison distinguishes between using AI as a "typing assistant" (comprehension maintained, skill preserved) vs. vibe coding (comprehension abandoned, skill atrophied). The distinction is useful for framing the atrophy risk, though it is practitioner argument, not a study.

**The longitudinal gap.** No published long-term study tracks measured decline in developer skill from sustained AI use. All "deskilling" claims are cross-sectional or theoretical. This is the single biggest evidentiary gap in the entire failure-mode literature. The concern is well-motivated by mechanism (B1, D1, openreview WpL9rAd0K7) but not yet demonstrated longitudinally.

**HCI / cognitive science connection.**
- **Bainbridge (1983), "Ironies of Automation"** (*Automatica*) — the human kept in the loop for oversight is denied the practice that would make their oversight competent. Direct structural parallel to agentic coding.
- **Parasuraman & Riley (1997)** — automation disuse and misuse taxonomy; skill atrophy is the long-run consequence of sustained disuse.
- **Cognitive load theory (Sweller)** — skills are built through effortful processing; removing effort removes the learning signal.

**Downstream consequences.** Developers who cannot work without AI assistance (vendor lock-in at the cognitive level); degraded ability to review AI output (the oversight loop fails because the overseer's skills have atrophied); junior developers who never build foundational skills; organizational fragility when AI tools are unavailable or wrong.

---

## PART 2 — Guardrails (separated by layer)

### Individual habits
- **Deliberate practice windows.** Reserve blocks of time for AI-free coding on non-critical tasks — the equivalent of a surgeon practicing sutures. `[ATROPHY]`
- **Author the hard parts.** Write debugging, algorithm design, and security-critical code by hand. Delegate only boilerplate and scaffolding. `[ATROPHY][FLOW]`
- **Batch supervision.** Rather than context-switching on every agent suggestion, batch reviews to dedicated review windows — reduces switch frequency. `[CONTEXT]`
- **Flow protection.** Use AI for setup/scaffolding *before* entering deep work; avoid invoking agents mid-flow. `[FLOW]`

### Team conventions
- **Skill audit in retrospectives.** Periodically ask: "What can we no longer do without AI?" Flag atrophied skills before they become critical gaps. `[ATROPHY]`
- **Pair on agent output.** Keep a second human in the loop for non-trivial agent work — distributes the supervision cost and maintains two people's mental models. `[CONTEXT][ATROPHY]`
- **Review-for-understanding norm.** "I couldn't follow this" is a valid block — forces the reviewer to maintain comprehension skills. `[ATROPHY][CONTEXT]`

### Tooling
- **Explicit context files (CLAUDE.md / AGENTS.md pattern).** Writing constraints and goals explicitly restores authorial engagement and reduces the "almost-right" correction loop. `[FLOW][CONTEXT]`
- **Friction at delegation boundaries.** Require explicit human approval before agent proceeds to next file/module — reduces the diff-size that triggers the worst context-switch costs. `[CONTEXT]`

---

## PART 3 — Patterns Across the Three Modes

1. **The three modes form a reinforcing loop.** Context-switching degrades flow → degraded flow reduces the quality of supervision → reduced supervision quality accelerates skill atrophy → atrophied skills make context-switching more costly (you can't quickly re-orient to code you don't fully understand).

2. **The "almost-right" problem is the common mechanism.** The 66% of developers spending more time fixing almost-right code (SO 2025) is simultaneously a context-switch cost, a flow-killer, and a skill-atrophy accelerant (you practice fixing AI errors, not building original solutions).

3. **Flow evidence is genuinely mixed.** DORA 2025 shows AI *increases* individual flow for some developers. The failure mode is not universal — it depends on what the developer values (output vs. craft) and what they delegate (boilerplate vs. core logic). Synthesis should resist a single narrative.

4. **Skill atrophy is the most consequential but least evidenced.** The mechanism is well-supported (B1, D1, openreview WpL9rAd0K7); the longitudinal outcome is not. This is the highest-priority gap for future research.

---

## PART 4 — Open Questions & Thin Evidence

- **Thin: no longitudinal skill-atrophy study exists.** The single biggest gap in the literature. All deskilling claims are cross-sectional or mechanistic inference.
- **Flow evidence is mixed and self-reported.** DORA 2025 (flow up) vs. HN thread (flow lost) — these may measure different populations, different task types, or different definitions of flow. No controlled study of flow-state frequency/depth in AI-assisted vs. unassisted coding was found.
- **Context-switching cost is inferred, not measured.** The SO "almost-right" burden and METR's 9% review overhead are proxies; no study directly measures switch frequency or switch cost in agentic workflows.
- **openreview WpL9rAd0K7 needs verification.** The RCT on comprehension loss is the strongest direct evidence for atrophy in a coding context, but the openreview link should be confirmed against a published/accepted venue before citing as peer-reviewed.
- **NSF/PAR 10677745 needs verification.** The "Fast and Spurious" citation should be confirmed against the NSF PAR database for full author list, venue, and exact figures.

---

### Source ledger (verifiable)

| # | Source | Mode | Grade |
|---|--------|------|-------|
| 1 | Stack Overflow 2025 (>49,000 devs) | CONTEXT, FLOW | STRONG (survey) |
| 2 | NSF/PAR 10677745 — "The Fast and Spurious" (Afroz et al., 2025) | CONTEXT, ATROPHY | MODERATE ⚠ verify |
| 3 | arXiv 2507.08149 — coding agent controlled study (2025) | CONTEXT | MODERATE ⚠ verify |
| 4 | METR RCT (2025) — 9% review overhead, 56% major edits | CONTEXT, FLOW, ATROPHY | STRONG |
| 5 | HN "Lost Joy of Programming" thread (2025) | FLOW | MODERATE (community signal) |
| 6 | DORA 2025 — flow up, stability still down | FLOW | MODERATE (survey) |
| 7 | MSR+CMU Lee et al. 2025 (CHI) — critical thinking reduction | ATROPHY | STRONG (peer-reviewed) |
| 8 | openreview WpL9rAd0K7 — comprehension loss RCT (2025) | ATROPHY | MODERATE ⚠ verify venue |
| 9 | Parasuraman & Manzey 2010 — complacency/bias | ATROPHY | STRONG (foundational) |
| 10 | Simon Willison — vibe coding (2025) | ATROPHY, FLOW | WEAK (practitioner) |
| 11 | Bainbridge 1983 — Ironies of Automation | ATROPHY | STRONG (foundational, not dev-specific) |
