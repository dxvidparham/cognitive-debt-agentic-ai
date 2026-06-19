# Mitigating Context Switching

> Failure mode: [failure-mode.md](./failure-mode.md)

The context-switching burden is structural. Every agent invocation is a mode switch: you exit developer mode, enter reviewer mode, parse the diff, decide, fix what's wrong, and try to re-enter where you left off. The mitigation is not to review faster — it's to reduce the frequency and size of switches, and to make the ones that remain deliberate rather than reactive.

---

## Baseline

### Hard limit: three concurrent agents

Don't run more than three agentic streams at once. This is not a soft guideline — it's a cognitive capacity constraint. The fourth stream doesn't add 25% more output. It degrades the quality of everything, including the three streams you were already managing.

BCG research found a sharp productivity cliff at four concurrent streams. Error rates climb. Output quality drops. The human working memory cannot supervise that many parallel contexts without degrading.

### Batch your supervision

Instead of reviewing every agent output the moment it arrives, batch reviews into dedicated windows. Treat agent output review as scheduled work, not an interrupt.

Reactive context-switching (agent finishes → you drop what you're doing → you review) versus planned review windows (you finish your current task → you review the batch) is the difference between a fragmented session and a productive one.

### Scope the agent tightly before you start

The larger the diff, the more expensive the review. The fix isn't faster reviewing — it's smaller diffs.

Before starting any agent session, write down exactly what you want it to do and what files it should touch. A well-scoped agent returns a reviewable diff. An under-scoped agent returns a sprawling change that takes longer to review than it would have taken to write.

---

## AGENTS.md snippet

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

## Experimental tools

> ⚗️ These are tools one developer built and runs in production to test whether the theory holds in practice. They are not prescriptions — they are included because real attempts at mitigation are more useful than abstract recommendations.

### `ask-questions-if-underspecified` skill

**What it does:** Mandatory clarification gate before any work begins. If a request has ambiguous objectives, unclear scope, or multiple valid interpretations with significantly different effort profiles, the agent asks 1–5 targeted questions before proceeding. It cannot start work on an underspecified task.

**Why it matters:** Underspecified tasks produce large, sprawling diffs — exactly the kind that maximise context-switching cost. Clarifying upfront reduces the diff size and the review burden.

**Targets:** The "almost-right" problem (45% of developers, SO 2025) — which is largely a scoping failure.

**Source:** Trail of Bits · [trailofbits/skills](https://github.com/trailofbits/skills/tree/main/plugins/ask-questions-if-underspecified) (adapted).

---

### MemPalace + Engram MCP servers

**What they do:** Persistent memory across sessions. MemPalace (ChromaDB + knowledge graph) and Engram (SQLite/FTS5) both store architectural decisions, debugging patterns, and prior work. At session start, the agent loads this context before responding to anything.

**Why it matters for context-switching:** A significant portion of context-switching cost is re-establishing context that existed in a previous session. Persistent memory eliminates the "start from scratch" tax on every new session — the agent already knows what was decided, what was tried, and what the current state is.

**Status:** Both running in production. Being evaluated in parallel — one will be retired after ~1 month.

**Source:** MemPalace · Engram. (Sources not publicly confirmed — links omitted to avoid incorrect attribution.)

---

### Concurrency cap in AGENTS.md (global policy)

**What it does:** A hard rule in the global AGENTS.md: never run more than 3 concurrent agent streams. If a fourth is needed, one of the three must complete first.

**Why it matters:** The cap is enforced at the instruction level — the agent knows the rule and applies it to its own orchestration decisions. It's not a UI constraint; it's a cognitive hygiene policy baked into the agent's operating instructions.

**Status:** Active in global AGENTS.md. Text:

```markdown
**Concurrency cap:** Never run more than 3 concurrent agent streams. If a fourth
is needed, one of the three must complete first. Supervising 4+ concurrent agents
produces AI brain fry — cognitive load that degrades review quality across all streams.
```

**Source:** Custom, informed by BCG/Built In research on concurrent agent management.

---

→ Back: [Overview](../overview.md) · Next: [05-flow-state-disruption/mitigation.md](../05-flow-state-disruption/mitigation.md)
