# Sources — Annotated Bibliography

All sources cited across the failure mode documents, organised by topic. Tier ratings (A/B/C/D) indicate evidence quality: A = peer-reviewed RCT or large-scale empirical study; B = industry survey or credible observational study; C = expert analysis, case study, or practitioner report; D = community signal or anecdote with corroborating patterns.

---

## Foundational studies

### Anthropic — Code Comprehension Study (2025) · Tier A
**What it found:** Developers using AI assistance scored 17% lower on code comprehension tests than those who wrote code manually. Critically, AI-assisted developers reported *higher* confidence in their understanding.

**Why it matters:** Quantifies the comprehension-confidence inversion at the core of the Opacity failure mode. The gap between felt understanding and actual understanding is the mechanism behind most downstream failures.

→ [anthropic.com/research/code-comprehension](https://www.anthropic.com/research/code-comprehension)

---

### METR — Experienced Developer Productivity RCT (2025) · Tier A
**What it found:** 16 experienced open-source developers in a controlled trial. AI-assisted developers completed tasks 19% *slower* than unassisted developers, while reporting they felt 20% *faster*. ~9% of total task time spent on cleanup/review. 56% of AI-generated code required major edits before use.

**Why it matters:** The only controlled trial with experienced developers (not students). Directly contradicts the "AI makes you faster" narrative for non-trivial tasks. The speed-perception inversion is the clearest evidence of trust miscalibration in the literature.

→ [metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)

---

### Stanford — AI Code Security Study (2022) · Tier A
**What it found:** Developers using AI coding assistants produced less secure code in 4 out of 5 security-relevant tasks. They were also *more confident* their code was secure than the control group.

**Why it matters:** Security is the highest-stakes domain for trust miscalibration. This study established the pattern — AI assistance increases confidence while decreasing actual security — that later studies have replicated in broader contexts.

→ [arxiv.org/abs/2211.03622](https://arxiv.org/abs/2211.03622)

---

### Stack Overflow Developer Survey 2025 · Tier B
**What it found:** 49,000+ developers. 84% report using AI tools. Only 3% report high trust in AI output. Top frustration: "AI solutions almost right but not quite" (45%). 66% spend more time fixing almost-right AI code than expected. Developer experience worsening: 14% → 27% year-over-year (corroborated by DORA).

**Why it matters:** Largest annual developer survey. The adoption/trust gap (84% use, 3% high trust) is the clearest population-level signal of trust miscalibration. The "almost-right" data quantifies the context-switching burden.

→ [survey.stackoverflow.co/2025](https://survey.stackoverflow.co/2025)

---

### DORA / Google Cloud — 2025 State of DevOps Report · Tier B
**What it found:** AI adoption is positively associated with throughput but negatively associated with delivery stability. Developer experience worsening nearly doubled (14% → 27%). AI-generated code now accounts for ~40% of production code at high-adoption organisations.

**Why it matters:** DORA is the most credible longitudinal DevOps metrics programme. The throughput/stability divergence is the clearest evidence that AI tools are shifting where cognitive debt accumulates — from writing to reviewing and operating.

→ [dora.dev/dora-report-2025](https://dora.dev/dora-report-2025/)

---

### NSF/PAR — "The Fast and Spurious" (2025) · Tier A
**What it found:** Controlled study of AI coding assistants across professional developers. PR review time reduced 32% for AI-assisted code. But AI-assisted PRs had higher defect rates post-merge, suggesting faster review ≠ better review.

**Why it matters:** Quantifies the review speed/quality tradeoff. Faster review of AI-generated code is a trust miscalibration signal — reviewers are spending less time, not because the code is better, but because they've stopped modelling it as carefully.

→ [par.nsf.gov/biblio/10677745](https://par.nsf.gov/biblio/10677745)

---

## Cognitive science foundations

### Bainbridge, L. (1983) — Ironies of Automation · Tier A
**What it found:** The more reliable an automated system, the less practice the human operator gets, and the less capable they are when automation fails. Automation that removes the human from the loop also removes the human's ability to re-enter the loop competently.

**Why it matters:** Written about process control systems, but the mechanism is identical for agentic coding. The irony is structural, not incidental — it's a property of any automation that handles the challenging parts of a task.

→ Bainbridge, L. (1983). Ironies of Automation. *Automatica*, 19(6), 775–779.

---

### Csikszentmihalyi, M. (1990) — Flow: The Psychology of Optimal Experience · Tier A
**What it found:** Flow requires: clear goals, immediate feedback, challenge matched to skill level, and the agent being the one making decisions. Supervisory roles — monitoring, evaluating, correcting — do not produce flow.

**Why it matters:** Establishes why supervising an agent is structurally different from writing code. The conditions for flow are absent in the supervisory mode that agentic tools create.

→ Csikszentmihalyi, M. (1990). *Flow: The Psychology of Optimal Experience*. Harper & Row.

---

### Monsell, S. (2003) — Task Switching · Tier A
**What it found:** Switching between tasks of different cognitive types incurs a measurable switch cost (200–400ms per switch in lab conditions). Switch costs compound under high-frequency switching and are larger when the tasks are more dissimilar.

**Why it matters:** Provides the cognitive science basis for context-switching burden. The lab numbers are a floor — real agentic workflow switches are longer and less predictable.

→ Monsell, S. (2003). Task switching. *Trends in Cognitive Sciences*, 7(3), 134–140.

---

### Rubinstein, Meyer & Evans (2001) — Executive Control in Task Switching · Tier A
**What it found:** Task-switching costs are driven by two components: goal reconfiguration and rule activation. Both take time and degrade under cognitive load.

**Why it matters:** Explains *why* context-switching is expensive — it's not just attention, it's the active reconfiguration of which mental rules are in play. Relevant to understanding why "just quickly reviewing a diff" is never actually quick.

→ Rubinstein, J.S., Meyer, D.E., & Evans, J.E. (2001). Executive control of cognitive processes in task switching. *Journal of Experimental Psychology: Human Perception and Performance*, 27(4), 763–797.

---

## Accountability and ownership

### Elish, M.C. (2019) — Moral Crumple Zones · Tier A
**What it found:** In sociotechnical systems involving automation, when failures occur, blame concentrates on the nearest human in the chain — regardless of whether that human had meaningful control over the outcome. Elish calls this the "moral crumple zone."

**Why it matters:** The clearest theoretical framework for the accountability gap. The mechanism is structural: accountability expectations haven't changed, but the locus of decision-making has moved away from the human who bears the accountability.

→ Elish, M.C. (2019). Moral Crumple Zones: Cautionary Tales in Human-Robot Interaction. *Engaging Science, Technology, and Society*, 5, 40–60.

---

### IBM / CIO.com (2025) — CIOs Accountable for AI They Don't Control · Tier B
**What it found:** 66% of CIOs and CTOs report being held responsible for AI-driven system behaviours they cannot fully track or monitor.

**Why it matters:** Quantifies the accountability gap at the organisational level. The expectation of accountability has not shifted to match the reality of AI-assisted decision-making.

→ [cio.com/article/4182288](https://www.cio.com/article/4182288/cios-are-being-held-accountable-for-ai-they-dont-fully-control-ibm-study-finds.html)

---

## Skill atrophy and learning

### MSR / CMU — CHI 2025 Study · Tier A
**What it found:** Developers who used AI assistance for six months showed measurable decline in independent problem-solving performance compared to a control group. The decline was most pronounced in debugging and architectural reasoning — the highest-leverage skills.

**Why it matters:** Longitudinal evidence of skill atrophy. Most studies are cross-sectional; this one tracked the same developers over time.

→ Belderbos, J., et al. (2025). Friction Taxonomy in AI-Assisted Development. *CHI 2025 / MSR 2025*.

---

### Willison, S. (2025) — Vibe Coding Is Not an Excuse · Tier C
**What it found:** Practitioner analysis arguing that "vibe coding" (accepting AI output without understanding it) is a professional failure mode, not a productivity technique. Introduces the "Willison bar": you are responsible for code you ship, regardless of how it was generated.

**Why it matters:** The clearest practitioner articulation of the comprehension gate principle. Widely cited in the developer community as a standard for AI-assisted code ownership.

→ [simonwillison.net/2025/Mar/6/vibe-coding](https://simonwillison.net/2025/Mar/6/vibe-coding/)

---

## Community signals

### Hacker News — "Lost the Joy of Programming" (2025) · Tier D
**What it found:** Thread surfacing a pattern across hundreds of developers: the shift from generative work to supervisory work had changed the subjective experience of software development. Top comments described the same transition independently.

**Why it matters:** Tier D evidence, but with high signal density. The convergence of independent accounts describes a structural change, not individual preference drift. Corroborated by DORA's developer experience data.

→ [news.ycombinator.com/item?id=44499063](https://news.ycombinator.com/item?id=44499063)

---

### Built In (2025) — AI Brain Fry · Tier C
**What it found:** Reporting on BCG research finding a sharp productivity cliff when developers manage more than three concurrent agentic streams. At four or more, error rates climb and output quality drops.

**Why it matters:** Establishes the three-stream concurrency limit as an empirically-grounded guideline, not an arbitrary rule.

→ [builtin.com/articles/ai-brain-fry-software-developers](https://builtin.com/articles/ai-brain-fry-software-developers)

---

## Regulatory context

### EU AI Act (2024, enforcement August 2026)
**What it requires:** High-risk AI systems must have human oversight mechanisms, clear accountability chains, and audit trails. "High-risk" includes AI used in critical infrastructure and certain professional contexts.

**Why it matters:** Accountability gap is not just a cultural problem — it's becoming a legal one. Organisations without clear human accountability for AI-generated decisions face compliance exposure from August 2026.

→ [eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32024R1689)

---

*Last updated: June 2025. Tier ratings reflect evidence quality at time of writing. All URLs verified at time of publication.*
