# Evidence Base: Cognitive & Psychological Effects of AI-Assisted / Agentic Software Development

**Purpose:** Empirical backbone for a synthesis report on developer cognitive failure modes in agentic workflows.
**Scope:** Peer-reviewed studies, RCTs, and large-N industry research. Marketing blog posts are excluded or quarantined.
**Verification status:** Every entry below was located and cross-checked against its primary source (arXiv / ACM / official report page / journal). Figures marked "exact" were seen verbatim in the source. Anything unconfirmed is labeled explicitly.
**Compiled:** 2026-06-09.

---

## Reliability Tiers (legend)

| Tier | Meaning |
|------|---------|
| **A — Strong** | RCT or large-N (thousands) with controlled/causal design and published method. |
| **B — Solid** | Peer-reviewed controlled user study or large survey; smaller N or self-report, but rigorous. |
| **C — Suggestive** | Industry survey, observational, or self-report at scale; directional, not causal. |
| **D — Foundational/Theoretical** | Seminal review or model; not about AI coding specifically but the cited cognitive mechanism. |

---

## TIER A — Strongest causal / large-N evidence

### A1. METR (2025) — AI tools made experienced OSS devs ~19% SLOWER (RCT)
- **Title:** *Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*
- **Org/Authors:** METR (Model Evaluation & Threat Research); lead author Joel Becker et al.
- **Year:** 2025 (published 2025-07-10)
- **URLs:**
  - Blog: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
  - Paper (PDF): https://kwfoundation.org/wp-content/uploads/2025/07/Early_2025_AI_Experienced_OS_Devs_Study.pdf
  - Data/code: https://github.com/METR/Measuring-Early-2025-AI-on-Exp-OSS-Devs/
- **Method:** Randomized controlled trial. 16 experienced developers, 246 real issues from their own mature repos (avg ~23k stars, ~1.1M LOC, ~5 yrs prior experience). Each issue randomly assigned AI-allowed (primarily Cursor Pro + Claude 3.5/3.7 Sonnet) or AI-disallowed. Screen recordings; 143 hrs (29%) hand-labeled at ~10s resolution. Conducted Feb–Jun 2025.
- **Key finding (exact):** AI-allowed issues took **19% longer** (point estimate of slowdown = **0.188**; i.e. time-with-AI / time-without-AI − 1). 95% CI (clustered by dev): **+1.6% to +39%** — entirely on the slowdown side. Developers **forecast 24% speedup** beforehand and **still estimated 20% speedup afterward** despite being measurably slower. ML experts forecast 38% speedup, economics experts 39% — both badly wrong. Devs accepted **<44%** of AI suggestions; 56% reported major edits to clean up AI code; ~9% of time spent reviewing/cleaning AI output. 136 issues completed AI-allowed vs. 110 AI-disallowed.
- **Failure modes:** perception-vs-reality gap (trust miscalibration), over-reliance, opacity/comprehension loss on mature codebases, review-and-cleanup burden, interrupted flow.
- **Caveats:** Small N (16 devs) — but 246 tasks give a substantial dataset. Narrow population (expert devs, highly familiar large repos) — explicitly NOT generalizable to juniors or unfamiliar code. Early-2025 tooling snapshot (pre-agentic-IDE maturity). One dev with >50 hrs Cursor experience showed positive speedup → possible high skill ceiling. METR states experimental artifacts can't be fully ruled out but the effect is robust across analyses.

### A2. Peng, Kalliamvakou et al. / GitHub–Microsoft (2023) — Copilot RCT, task done 55.8% faster
- **Title:** *The Impact of AI on Developer Productivity: Evidence from GitHub Copilot*
- **Authors/Org:** Sida Peng, Eirini Kalliamvakou, Peter Cihon, Mert Demirer (GitHub / Microsoft / MIT)
- **Year:** 2023 (arXiv 2023-02-13)
- **URLs:** https://arxiv.org/abs/2302.06590 · GitHub writeup: https://github.blog/news-insights/research/research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/
- **Method:** RCT. 95 professional devs recruited via Upwork; randomized to Copilot (45) vs. control (50); 35 each completed. Task: write an HTTP server in JavaScript as fast as possible. Standardized, test-scored task.
- **Key finding (exact):** Treatment group completed **55.8% faster** (71.17 min vs. 160.89 min; p=0.0017; 95% CI [21%, 89%]). Completion rate 78% vs. 70% (not statistically significant). Self-estimated gain was ~35% — an **underestimate** of the actual 55.8% (opposite direction from METR's overestimate). Less-experienced/older/higher-volume coders benefited most.
- **Failure modes:** Counter-evidence to slowdown claims; relevant to the productivity-up vs. slowdown contradiction. Speaks to skill-leveling (juniors gain most).
- **Caveats:** Toy greenfield task, not real codebase work — the opposite end of the spectrum from METR. Population skews lower-income/early-career (India/Pakistan, median income $10–19k). Conducted by the vendor (GitHub/MS) — note conflict of interest, though published as a working paper.

### A3. Cui, Demirer, Peng et al. / MIT (2024) — Three field RCTs, 4,867 devs, +26% tasks completed
- **Title:** *The Effects of Generative AI on High-Skilled Work: Evidence from Three Field Experiments with Software Developers*
- **Authors:** Zheyuan (Kevin) Cui, Mert Demirer, Sonia Jaffe, Leon Musolff, Sida Peng, Tobias Salz (MIT et al.)
- **Year:** 2024/2025 working paper
- **URL:** https://economics.mit.edu/sites/default/files/inline-files/draft_copilot_experiments.pdf
- **Method:** Three large RCTs at Microsoft, Accenture, and an anonymous Fortune 100 firm; **4,867 developers** randomly granted Copilot access over 2–8 months as part of normal business. Instrumental-variable regression.
- **Key finding (exact):** Pooled **+26.08% increase in completed tasks (weekly PRs)** (SE 10.3%); +13.55% commits (SE 10.0%); +38.38% builds (SE 12.55%). Less-experienced devs adopted more and gained more. Authors explicitly note this real-world effect is **far smaller** than the 55.8% lab result of Peng et al. (2023).
- **Failure modes:** Productivity-positive counterweight at scale; reinforces "juniors gain most"; relevant to the contradiction section.
- **Caveats:** Output metrics (PRs/commits/builds) are activity proxies, not quality or value. "Noisy" per the authors. One Accenture sub-experiment turned negative/insignificant under conservative imputation. Vendor-adjacent (Microsoft/GitHub data).

---

## TIER B — Solid peer-reviewed controlled studies & rigorous surveys

### B1. Lee, Sarkar et al. / Microsoft Research + CMU (2025, CHI) — GenAI & critical thinking / cognitive offloading
- **Title:** *The Impact of Generative AI on Critical Thinking: Self-Reported Reductions in Cognitive Effort and Confidence Effects From a Survey of Knowledge Workers*
- **Authors:** Hao-Ping (Hank) Lee (CMU), Advait Sarkar, Lev Tankelevitch, Ian Drosos, Sean Rintel, Richard Banks, Nicholas Wilson (Microsoft Research)
- **Year:** 2025, CHI '25, ACM Article 1121
- **URLs:** https://www.microsoft.com/en-us/research/publication/the-impact-of-generative-ai-on-critical-thinking-self-reported-reductions-in-cognitive-effort-and-confidence-effects-from-a-survey-of-knowledge-workers/ · DOI: https://doi.org/10.1145/3706598.3713778 · PDF: https://www.microsoft.com/en-us/research/wp-content/uploads/2025/01/lee_2025_ai_critical_thinking_survey.pdf
- **Method:** Survey of **319 knowledge workers** (weekly+ GenAI users) on Prolific; **936 first-hand task examples**; critical thinking scored across Bloom's six categories. Regression on confidence/effort.
- **Key finding (exact):** **Higher confidence in GenAI → less critical thinking** (β = −0.69, p < 0.001); higher self-confidence → more critical thinking (β = 0.26, p = 0.026). Majority report **reduced perceived effort** for critical-thinking activities when using GenAI: "much less / less effort" in 72% (Knowledge), 79% (Comprehension), 69% (Application), 72% (Analysis), 76% (Synthesis), 55% (Evaluation). GenAI shifts work **from execution to oversight** (verification, integration, task stewardship).
- **Failure modes:** cognitive offloading / skill atrophy, over-reliance, accountability gap (oversight burden), trust miscalibration.
- **Caveats:** Self-report of a hard-to-observe construct ("enaction" of critical thinking, not critical thinking itself). Cross-sectional — no longitudinal/causal claim on actual skill decay. Knowledge workers broadly, not devs specifically. Microsoft-authored.

### B2. Perry, Srivastava, Kumar, Boneh / Stanford (2023, ACM CCS) — Users write LESS secure code with AI + overconfidence
- **Title:** *Do Users Write More Insecure Code with AI Assistants?*
- **Authors:** Neil Perry*, Megha Srivastava* (equal), Deepak Kumar (Stanford / UC San Diego), Dan Boneh — Stanford University
- **Year:** arXiv 2022-11-07; published ACM CCS '23
- **URLs:** https://arxiv.org/abs/2211.03622 · DOI: https://doi.org/10.1145/3576915.3623157 · PDF: https://kumarde.com/papers/codex.pdf
- **Method:** Controlled user study. **47 participants**, 5 security-related tasks across Python/JS/C, with vs. without an AI assistant (OpenAI codex-davinci-002). Logistic regression controlling for prior experience, security exposure, student status.
- **Key finding (exact):** Participants with AI access wrote **significantly less secure code** on **4 of 5 tasks**, yet were **more likely to believe their code was secure** (overconfidence). Correctness 67% (AI) vs. 79% (control); statistically significant insecurity increase (e.g. p=0.017 on one task; p=0.018 trivial-cipher use). Those who **trusted the AI less and engineered prompts more carefully wrote more secure code**.
- **Failure modes:** automation bias / trust miscalibration, accountability gap, security-specific over-reliance, prompt-engineering burden (better prompting → safer output).
- **Caveats:** N=47, Codex-era model (now dated vs. frontier models). Lab tasks, not production. Single AI model.

### B3. (Foundational, applied) — see D1 below for the automation-bias mechanism underpinning B2/A1's overconfidence findings.

---

## TIER C — Large-N industry surveys (directional, mostly self-report)

### C1. DORA 2024 — *Accelerate State of DevOps Report* (Google) — AI helps individuals, HURTS delivery
- **Org:** DORA / Google Cloud. **Year:** 2024 (10th edition).
- **URLs:** https://dora.dev/research/2024/dora-report/ · Google blog: https://cloud.google.com/blog/products/devops-sre/announcing-the-2024-dora-report · Errata: https://dora.dev/research/2024/errata/
- **Method:** Global survey, **>39,000 professionals** (some sources cite ~3,000 for the AI-specific modeling subset per TechTarget) + interviews.
- **Key findings (exact):** AI adoption increases individual productivity, flow, job satisfaction — BUT for each ~25% increase in AI adoption, estimated **−1.5% delivery throughput** and **−7.2% delivery stability**. **39% of respondents reported low/no trust** in AI-generated code. **NOTE (errata v.2024.2):** the burnout figure in Figure 18 was corrected to **−9.9%** (AI adoption associated with reduced burnout). Verify the exact metric framing against the corrected PDF (v.2024.3).
- **Failure modes:** trust miscalibration (39% distrust), accountability/stability gap, throughput regression — direct contradiction of A2/A3.
- **Caveats:** Self-reported survey; "AI adoption" is correlational, not assigned. Metric framing changed between 2024 and 2025, complicating year-over-year comparison (see Contradictions). Multiple errata — cite v.2024.3.

### C2. DORA 2025 — *State of AI-assisted Software Development* (Google) — throughput recovers, stability still down, trust still low
- **Org:** DORA / Google Cloud. **Year:** 2025 (published 2025-09-23).
- **URLs:** https://dora.dev/dora-report-2025/ · Google blog: https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report · InfoQ: https://www.infoq.com/news/2025/09/dora-state-of-ai-in-dev-2025/
- **Method:** ~**5,000 technology professionals** + 100+ hrs qualitative interviews.
- **Key findings (exact):** AI adoption now **90%** (up from 76% in 2024). **>80%** report increased productivity; **59%** report improved code quality; **71%** of code-writers use AI to write new code. Crucially, the 2024 negative throughput relationship **reversed to positive** in 2025 ("AI accelerates development"), but AI adoption **still negatively related to delivery stability**. Trust: **30%** report little/no trust (down from 39.2% in 2024); only **~30%** (Google blog) trust outputs as accurate. Central thesis: **"AI is an amplifier"** — magnifies existing org strengths/weaknesses; the "fail fast, fix fast" hypothesis to offset instability did **not** hold.
- **Failure modes:** persistent stability/accountability gap, trust miscalibration, amplification of dysfunction.
- **Caveats:** Self-report. Metric methodology changed from 2024 ("measured differently," per DORA lead Nathen Harvey) — year-over-year throughput reversal may be partly methodological, not purely real-world learning. Correlational.

### C3. Stack Overflow Developer Survey 2025 — trust in AI accuracy at record low
- **Org:** Stack Overflow. **Year:** 2025.
- **URLs:** https://survey.stackoverflow.co/2025 · Press: https://stackoverflow.co/company/press/archive/stack-overflow-2025-developer-survey/ · Blog: https://stackoverflow.blog/2025/12/29/developers-remain-willing-but-reluctant-to-use-ai-the-2025-developer-survey-results-are-here/
- **Method:** **>49,000 developers**, 177 countries.
- **Key findings (exact):** Adoption: **84%** use or plan to use AI tools (up from 76% in 2024) — the public blog/leaders TL;DR also cites 80% currently using. **Trust the accuracy: 33%** (highly 3.1% + somewhat 29.6%); **distrust: ~46%** (somewhat 26.1% + highly 19.6%). Only **3% "highly trust"** output. Experienced devs most cautious (highest "highly distrust" ~20%, lowest "highly trust" 2.6%). Top frustration (**45%**): "AI solutions that are almost right, but not quite"; **66%** say they spend more time fixing "almost-right" AI code; **75%** still ask a human when they don't trust the AI's answer. Favorability fell 72%→60% YoY; accuracy trust 40%→29% YoY.
- **Failure modes:** trust miscalibration / distrust, "almost-right" review burden (accountability + comprehension), context-switching to human help.
- **Caveats:** Self-selected respondents (SO's audience skews experienced/Western). Self-report. Trust ≠ measured quality. Slightly different headline percentages appear across SO's own pages (29% vs 33% trust) due to question framing (favorability vs accuracy-trust) — cite the specific question.

---

## TIER D — Foundational cognitive-science / HCI mechanisms (not AI-coding-specific, but the cited lineage)

### D1. Parasuraman & Manzey (2010) — *Complacency and Bias in Human Use of Automation: An Attentional Integration*
- **Authors:** Raja Parasuraman (George Mason Univ.), Dietrich H. Manzey (TU Berlin).
- **Venue/Year:** *Human Factors*, Vol. 52(3), June 2010, pp. 381–410. DOI: 10.1177/0018720810376055.
- **URLs:** https://journals.sagepub.com/doi/10.1177/0018720810376055 · PubMed: https://pubmed.ncbi.nlm.nih.gov/21077562/
- **Method:** Integrative review + theoretical model of empirical automation studies.
- **Key findings:** Automation **complacency** arises under multi-task load when manual tasks compete for attention; found in **both novices and experts** and **cannot be overcome by simple practice**. Automation **bias** produces both omission and commission errors with imperfect decision aids; occurs in novices and experts, in individuals and teams, and **cannot be prevented by training or instructions alone**. Both are attention-driven manifestations of the same automation-misuse phenomenon.
- **Failure modes:** automation bias / complacency / trust miscalibration — the canonical mechanism practitioners cite to explain B2 (over-trust) and A1 (perception gap). The "experts not immune" and "training doesn't fix it" findings are the most-cited points.
- **Caveats:** Pre-LLM, drawn from aviation/process-control/decision-support, not code. Mechanistic transfer to AI coding is plausible and widely cited but not directly measured here.

> Lineage note: The classic **"ironies of automation"** framing (Bainbridge, 1983, *Automatica*) and the **misuse/disuse** taxonomy (Parasuraman & Riley, 1997, *Human Factors*) are cited within D1 and are the deeper roots of the deskilling argument. **COULD NOT VERIFY independently in this pass** — flagged for follow-up if the synthesis wants primary citations rather than D1's secondary references.

---

## Where the research is CONTRADICTORY

1. **Productivity up vs. productivity down — the headline tension.**
   - Lab RCTs (A2: +55.8% on a greenfield task) and scaled field RCTs (A3: +26% tasks completed) show clear gains, *especially for juniors and unfamiliar/boilerplate work*.
   - METR's real-world RCT on **experts in mature, familiar codebases** (A1) shows the **opposite: −19%**.
   - These are **not necessarily in conflict** — they measure different populations and task types. The reconcileable reading: AI helps most where the human has *least* context (juniors, greenfield, boilerplate) and can *hurt* where the human already holds deep implicit context the model lacks (experts, large legacy repos). The danger is generalizing either result.

2. **Perception gap runs in BOTH directions.**
   - A2 (lab): devs *under*-estimated their gain (35% est. vs 55.8% real).
   - A1 (field): devs *over*-estimated their gain (20% est. vs −19% real).
   - Common thread: developer self-perception of AI productivity is **unreliable** in either direction — a measurement caution for any survey-based claim (i.e., undermines the self-report basis of C1–C3's productivity numbers).

3. **DORA 2024 vs DORA 2025 throughput reversal.**
   - 2024: AI adoption → −1.5% throughput.
   - 2025: AI adoption → *positive* throughput.
   - DORA attributes this to organizational learning, but explicitly notes the metric was "measured differently" — so part of the reversal may be methodological. Stability stayed negative in both years (the more stable finding).

4. **Trust trend direction.** SO survey shows accuracy-trust *falling* (40%→29%) while adoption *rises* (76%→84%) — the "adoption–trust gap." DORA shows distrust *easing slightly* (39%→30%). Both agree trust is low and adoption is near-universal; they disagree on the short-term trend.

---

## Where the research is THIN

- **Agentic (autonomous multi-step) workflows specifically.** Almost all rigorous evidence above concerns *autocomplete/chat assistants* (Copilot, Cursor, ChatGPT), not fully agentic coding agents. The agentic-specific cognitive load (prompt/context engineering, reviewing large autonomous diffs, accountability for code the dev never wrote) is **largely unmeasured by RCT** — currently inference from A1's review-burden data + B1's oversight-shift finding.
- **Longitudinal skill atrophy / deskilling in developers.** B1 and D1 motivate the *concern*, but no published long-term study tracks measured decline in developer skill from sustained AI use. All "deskilling" claims are cross-sectional or theoretical. **This is the single biggest gap.**
- **Flow-state interruption** from AI suggestions/context-switching: discussed anecdotally (A1's cleanup time, "almost-right" friction in C3) but no dedicated controlled measurement of flow disruption from AI tooling was verified.
- **Accountability gap** as a measured construct (who is responsible for AI-authored bugs): treated qualitatively (B1 "task stewardship," C3 "75% ask a human") but no rigorous study isolates it.
- **Prompt/context-engineering burden** as a cognitive cost: B2 shows better prompting → better outcomes, but the *load* of doing so isn't quantified.

---

## One-line reliability summary per source

| ID | Source | Tier | N | Design | Core number |
|----|--------|------|---|--------|-------------|
| A1 | METR 2025 | A | 16 devs / 246 tasks | RCT | −19% (slower) |
| A2 | Peng/GitHub 2023 | A | 95 / 70 completed | RCT | +55.8% faster (lab task) |
| A3 | MIT 3-firm 2024 | A | 4,867 devs | 3× field RCT | +26.08% tasks |
| B1 | MSR+CMU 2025 (CHI) | B | 319 workers | Survey + regression | Confidence-in-AI → less critical thinking (β=−0.69) |
| B2 | Stanford 2023 (CCS) | B | 47 | Controlled user study | Less secure code on 4/5 tasks + overconfidence |
| C1 | DORA 2024 | C | ~39,000 | Survey | −1.5% throughput, −7.2% stability, 39% distrust |
| C2 | DORA 2025 | C | ~5,000 | Survey + interviews | 90% adoption, throughput now +, stability still −, 30% distrust |
| C3 | Stack Overflow 2025 | C | 49,000+ | Survey | 46% distrust accuracy, 3% highly trust |
| D1 | Parasuraman & Manzey 2010 | D | review | Integrative review | Complacency/bias affect experts; training doesn't fix |

---

## Items I could NOT fully verify (flagged, not presented as fact)

- **Bainbridge (1983) "Ironies of Automation"** and **Parasuraman & Riley (1997) "Humans and Automation: Use, Misuse, Disuse, Abuse"** — referenced *within* D1 but not independently opened this pass. Real and canonical to my knowledge, but treat exact page/figure claims as unconfirmed until pulled directly.
- **DORA 2024 burnout figure:** the public Google blog emphasizes throughput/stability; the burnout number (−9.9%) comes from the **errata page** correcting Figure 18. Confirm the precise wording/metric against the corrected PDF (v.2024.3) before quoting.
- **DORA 2024 AI-specific sample size:** the headline >39,000 is the full survey; TechTarget reports the AI-modeling subset as ~3,000. Confirm which N attaches to the −1.5%/−7.2% estimates in the report body.
- Any "two years of learning in six weeks" Nigeria AI-tutor stat (quoted by an MSR researcher in Computerworld re B1) is **education, not developers** — out of scope and second-hand; do not use as dev evidence.
