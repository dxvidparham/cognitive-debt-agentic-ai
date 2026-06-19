# Skill Effectiveness Measurement Strategy

**Purpose**: Empirically track whether the ~34 installed agent skills improve outcomes over time.
**Audience**: Solo developer / small team. Not a research paper — a working protocol.
**Last updated**: 2026-06-10

---

## 1. What We're Measuring

The skills target 8 failure modes. Each failure mode maps to measurable outcomes:

| Failure Mode | Skills | Measurable Outcome |
|---|---|---|
| [OPACITY] | grill-me, audit-context-building, author-not-reviewer | Rework rate, PR comments requesting clarification |
| [ACCOUNTABILITY] | canary, pre-pr-internalization | Change failure rate, rollback frequency |
| [TRUST] | doubt-driven-development, pre-pr-internalization | Defect escape rate, post-merge bug rate |
| [CONTEXT] | ce-compound, ce-compound-refresh, source-driven-development | Hallucination rate in generated code, outdated-API usage |
| [FLOW] | (behavioral policy only) | Context switches per session, uninterrupted work blocks |
| [ATROPHY] | cognitive-gym, author-not-reviewer | Skill-without-agent success rate (see §4) |
| [PROMPT] | ce-compound, ce-compound-refresh | Stale-pattern recurrence, prompt ownership coverage |
| [ORCHESTRATION] | ask-questions-if-underspecified | Clarification rounds per task, abandoned tasks |

**Primary outcome variables** (collect these; everything else is secondary):
1. **Defect escape rate** — bugs reaching production / total bugs created
2. **PR cycle time** — commit to merge (median, p95)
3. **Rework rate** — PRs requiring >2 revision rounds
4. **Clarification rounds** — back-and-forth turns before implementation starts (proxy for [ORCHESTRATION])
5. **Skill-without-agent rate** — fraction of tasks where developer solved it before/without agent (proxy for [ATROPHY])

---

## 2. Measurement Design

### Design choice: Interrupted Time-Series (ITS)

You can't randomize yourself. ITS is the right quasi-experimental design for a single developer or team adopting a new practice:

- Measure outcomes for **T₀ weeks pre-intervention** (baseline)
- Introduce intervention
- Measure outcomes for **T₁ weeks post-intervention**
- Fit segmented regression: detect both *level change* (immediate effect) and *slope change* (trend)

**Minimum durations:**

| Phase | Duration | Why |
|---|---|---|
| Baseline | 6 weeks | Needs enough data points for stable regression; shorter = noise |
| Intervention | 10 weeks | Skills need internalization time; effects often lag 2–3 weeks |
| Total | 16 weeks | ~4 months; one natural quarter |

### Staggered rollout (don't introduce all skills at once)

Introduce skill clusters one at a time, 2 weeks apart. This creates internal replication — each cluster is its own mini-ITS — and lets you attribute effects to specific skills rather than "the whole stack."

**Rollout order** (highest-signal first):

| Week | Cluster | Skills |
|---|---|---|
| 1–2 | Baseline only | None — measure everything |
| 3–4 | Pre-work gate | ask-questions-if-underspecified, grill-me |
| 5–6 | Review gate | pre-pr-internalization, doubt-driven-development |
| 7–8 | Debugging | systematic-debugging, verification-before-completion |
| 9–10 | Context refresh | ce-compound, ce-compound-refresh, audit-context-building |
| 11–12 | Accountability | canary, author-not-reviewer |
| 13–16 | Full stack + observation | All skills active; measure cumulative effect |

---

## 3. Data Collection Protocol

### What to instrument

**Automatic (zero friction):**
- Git log: commit frequency, PR open→merge time, branch lifetime
- Sentry / error tracker: production error rate, new issues per deploy
- OpenCode session logs: session duration, tool call counts, skill invocations (if logged)

**Manual (lightweight — max 2 min/day):**
- End-of-session log entry (see §3.1)
- Weekly 5-question survey (see §3.2)

### 3.1 End-of-session log

File: `~/.config/opencode/metrics/sessions.jsonl`

One JSON line per session:

```json
{
  "date": "2026-06-10",
  "session_id": "ses_...",
  "task_type": "feature|bugfix|refactor|review|research",
  "skills_fired": ["grill-me", "systematic-debugging"],
  "clarification_rounds": 2,
  "solved_before_agent": false,
  "rework_needed": false,
  "notes": "optional free text"
}
```

`solved_before_agent`: true if you diagnosed/solved the problem yourself before the agent gave an answer. This is the [ATROPHY] signal.

### 3.2 Weekly survey (5 questions, ~2 min)

File: `~/.config/opencode/metrics/weekly.jsonl`

```json
{
  "week": "2026-W24",
  "flow_blocks_per_day": 2.5,
  "frustration_score": 3,
  "skill_felt_forced": ["cognitive-gym"],
  "skill_felt_useful": ["grill-me", "pre-pr-internalization"],
  "notes": ""
}
```

`flow_blocks_per_day`: self-reported uninterrupted work blocks >90 min.
`frustration_score`: 1–5, how often did the agent slow you down this week.

### 3.3 PR tagging

Add a one-line comment to each PR description:

```
<!-- skills: grill-me, pre-pr-internalization | rework: no | clarity_rounds: 1 -->
```

This embeds the signal in the git history — queryable forever.

---

## 4. The [ATROPHY] Measurement Problem

[ATROPHY] is the hardest failure mode to measure because the signal is *absence* — the developer not needing the agent. Standard metrics won't capture it.

**Protocol: "Solve-first" discipline**

Before opening an agent session for any non-trivial task:
1. Spend 5–10 minutes attempting to solve it yourself (read the code, form a hypothesis, sketch an approach)
2. Log `solved_before_agent: true/false` in the session log
3. If false: note *why* (time pressure, genuinely unknown, lazy)

**Trend to watch**: Over 16 weeks, `solved_before_agent` rate should *increase* if cognitive-gym and author-not-reviewer are working. A flat or declining rate is a signal that [ATROPHY] is worsening despite the skills.

**Secondary signal**: Track `clarification_rounds` over time. If the agent needs fewer back-and-forth turns to understand your intent, your ability to specify problems clearly is improving.

**Friction taxonomy (Belderbos 2026).** Not all friction is equal. Before logging `solved_before_agent: false`, classify the friction type:

| Friction type | Examples | Expected delegation pattern |
|---|---|---|
| **Worth deleting** | Boilerplate, config syntax, scaffolding | Delegate freely — no atrophy risk |
| **Worth keeping** | Debugging, design decisions, code review | Preserve — this is where instinct forms |
| **Worth seeking out** | Reading unfamiliar internals, tracing below your abstraction level | Actively resist delegation |

Add a `friction_type` field to the session log (`deleted` / `kept` / `sought`). Over time, the ratio of `kept` + `sought` to `deleted` is a leading indicator of atrophy risk — a stack drifting toward all-`deleted` is a stack accelerating [ATROPHY] regardless of what the skills say.

---

## 5. Analysis Protocol

### At week 8 (mid-point check)

Run a quick sanity check — not a formal analysis:
- Are any metrics moving in the *wrong* direction? (e.g., PR cycle time increasing because skills add overhead)
- Are any skills consistently logged as `felt_forced`? Consider dropping or modifying them.
- Is `solved_before_agent` trending up, flat, or down?

**Decision rule**: If a skill appears in `felt_forced` for 3+ consecutive weeks and shows no measurable benefit, archive it. Don't carry dead weight.

### At week 16 (primary analysis)

**Step 1: Segmented regression on each primary metric**

For each metric, fit:
```
metric ~ time + intervention + time_after_intervention
```
- `intervention`: 0 before, 1 after
- `time_after_intervention`: 0 before, weeks elapsed after

Report:
- Level change (β_intervention): immediate effect at intervention point
- Slope change (β_time_after): trend change post-intervention
- 95% CI on each. If CI straddles zero, effect is inconclusive.

**Step 2: Effect size**

Cohen's d on (post-mean − pre-mean) / pooled SD:
- d < 0.2: negligible
- d 0.2–0.5: small but real
- d 0.5–0.8: medium — worth keeping
- d > 0.8: large — this skill is load-bearing

**Step 3: Skill-level attribution**

For each skill, filter sessions where it fired. Compare primary metrics for those sessions vs sessions where it didn't fire (controlling for task type). This is a within-subject comparison — not causal, but directionally useful.

**Step 4: Qualitative triangulation**

Review the `notes` fields from session logs and weekly surveys. Patterns in free text often explain what the numbers can't. "grill-me slowed me down on small tasks but caught a scope mistake on the auth refactor" is more actionable than a p-value.

---

## 6. Statistical Guardrails

These rules exist to prevent fooling yourself:

| Rule | Why |
|---|---|
| Pre-specify primary metrics before baseline starts | Post-hoc metric selection inflates false positives |
| One primary metric per failure mode | Testing 8 metrics at α=0.05 gives ~34% false positive rate without correction. Use Bonferroni: α_adjusted = 0.05/8 = 0.006 |
| Minimum 6 weeks baseline before any analysis | Shorter baselines mistake noise for trend |
| Pre-specify stopping rules | Early stopping inflates false positives 6× |
| Document all confounders before analysis | New job, team change, new codebase, major deadline — any of these can swamp the skill effect |
| Effect size required, not just p-value | With small N, p-values are unreliable. d > 0.3 is the minimum bar for "this matters" |

**Stopping rule**: If any metric shows a *negative* effect (d < −0.3, 95% CI entirely negative) after 8 weeks, pause that skill cluster and investigate before continuing.

---

## 7. Tooling

Minimal — don't build infrastructure for this. Use what exists.

**Data storage**: JSONL files in `~/.config/opencode/metrics/`. Plain text, git-trackable, queryable with `jq`.

**Analysis**: Python script (run at week 8 and week 16):

```python
# ~/.config/opencode/metrics/analyze.py
import json, statistics
from pathlib import Path

sessions = [json.loads(l) for l in Path("sessions.jsonl").read_text().splitlines()]

# Solved-before-agent rate over time
by_week = {}
for s in sessions:
    week = s["date"][:7]  # YYYY-MM
    by_week.setdefault(week, []).append(s["solved_before_agent"])

for week, vals in sorted(by_week.items()):
    rate = sum(vals) / len(vals)
    print(f"{week}: solve-first rate = {rate:.0%} (n={len(vals)})")
```

Extend as needed. Keep it under 100 lines — if analysis requires more, the data model is wrong.

**Git query for PR cycle time**:
```bash
git log --merges --format="%H %ai %s" | head -100
# Then compute open→merge delta from PR timestamps via gh CLI:
gh pr list --state merged --limit 100 --json number,createdAt,mergedAt \
  | jq '[.[] | {n: .number, days: ((.mergedAt | fromdateiso8601) - (.createdAt | fromdateiso8601)) / 86400}]'
```

---

## 8. Evaluation Cadence

| When | What |
|---|---|
| Before baseline starts | Pre-specify metrics, confounders, stopping rules. Write them down. |
| Week 1 | Start session log + weekly survey. Establish git/Sentry baseline query. |
| Week 3 | First skill cluster live. Check instrumentation is working. |
| Week 8 | Mid-point sanity check. Drop/modify skills with 3+ weeks of `felt_forced`. |
| Week 16 | Primary analysis. Segmented regression + effect sizes + qualitative review. |
| Week 20 | Decision: which skills stay, which get archived, what gets modified. |
| Quarterly | Re-run analysis on rolling 16-week window. Skills that consistently show d < 0.2 are candidates for archive. |

---

## 9. What Success Looks Like

**Minimum bar** (skill is worth keeping):
- Effect size d ≥ 0.3 on its primary outcome metric, OR
- Consistently appears in `felt_useful` (qualitative signal), OR
- Prevents at least one high-severity incident per quarter (anecdotal but real)

**Archive trigger** (skill is not pulling weight):
- Effect size d < 0.2 on primary metric after 16 weeks, AND
- Appears in `felt_forced` more than `felt_useful`, AND
- No memorable incident where it caught something important

**Red flag** (skill is actively harmful):
- Effect size d < −0.3 (metric worsened)
- Consistently cited as source of friction without corresponding benefit
- **Exception**: friction that maps to "worth keeping" or "worth seeking out" (Belderbos taxonomy) is *not* a red flag — it is the mechanism. A skill that slows you down on debugging is doing its job.
- → Archive immediately only when friction produces no learning signal. Don't wait for week 16.

---

## 10. Known Limitations

1. **N=1 problem**: Solo developer means no control group. ITS is the best available design but can't rule out confounders. Triangulate with qualitative evidence.
2. **Skill adoption ≠ skill compliance**: Logging that a skill fired doesn't mean it was followed rigorously. The session log's `notes` field is the only window into this.
3. **Hawthorne effect**: Measuring your own behavior changes it. The act of logging `solved_before_agent` may itself reduce [ATROPHY] — which is fine, but means the measurement is partly the intervention.
4. **Lag effects**: Some skills (cognitive-gym, author-not-reviewer) target slow-moving outcomes. 16 weeks may not be enough to see [ATROPHY] effects. Plan a 6-month follow-up.
5. **Metric gaming**: If you start optimizing for the metrics rather than the underlying outcomes, the measurement breaks. The qualitative layer (notes, weekly survey) is the check on this.

---

## Sources

- Forsgren, N., Humble, J., & Kim, G. (2018). *Accelerate*. IT Revolution Press. — DORA metrics
- Forsgren, N., Storey, M.-A., et al. (2021). "The SPACE of Developer Productivity." *ACM Queue*, 19(1). https://queue.acm.org/detail.cfm?id=3454124
- Gonçalves, P. W., et al. (2022). "Do explicit review strategies improve code review performance?" *Empirical Software Engineering*, 27(4). https://doi.org/10.1007/s10664-022-10123-8
- FutureAGI (2026). "A/B Testing LLM Prompts in 2026: A Stats Playbook." https://futureagi.com/blog/ab-testing-llm-prompts-best-practices-2026/
- Sentrial (2026). "Prompt A/B Testing That Catches Silent Failures." https://www.sentrial.com/blog/prompt-ab-testing-that-catches-silent-failures
- Belderbos, B. (2026). "How to Keep Your Developer Instincts When AI Writes the Code." https://belderbos.dev/blog/dont-delegate-the-friction/
- Faye, L. (2026). "Agentic Coding is a Trap." https://larsfaye.com/articles/agentic-coding-is-a-trap
