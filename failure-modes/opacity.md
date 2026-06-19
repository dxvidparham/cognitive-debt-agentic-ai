# Opacity — You ship code you don't understand

> *"When I let AI do too much, I feel disconnected to the codebase. It works. But I don't generally get how it works. Reviewing is not the same as understanding. The intention gap is real."*
>
> — dhung.dev, posted to r/ExperiencedDevs (2025), hundreds of corroborating replies

---

## What it is

Opacity is the gap between what your codebase does and what you actually understand about it.

It's not new. Developers have always inherited code they didn't write. But agentic AI accelerates it in a specific way: the code arrives correct-looking, passes tests, and gets merged — and the developer who approved it couldn't explain it to a colleague if asked.

The artifact exists. The mental model doesn't.

---

## Why agentic tools make it worse

With autocomplete tools like early Copilot, you still typed through every suggestion. Your fingers moved. You made micro-decisions. The code passed through your working memory.

Agentic tools are different. They generate across multiple files in autonomous loops and return a diff. Your job shifts from *author* to *reviewer of diffs*. And reviewing a diff is a fundamentally weaker path to understanding than writing the code yourself.

There's a well-established cognitive science principle behind this: the **generation effect** (Slamecka & Graf, 1978). Information you generate yourself is retained significantly better than information you merely read. Agentic workflows optimize away the generation step — which is exactly the step that builds the mental model.

---

## The evidence

**Anthropic coding skills RCT (2025)**
Developers who used AI assistance scored **17% lower** on conceptual understanding and code verification tests compared to developers who wrote the same code by hand. The largest performance gap was on **debugging questions** — the tasks that require the deepest mental model of how code actually works.
→ [anthropic.com/research/AI-assistance-coding-skills](https://www.anthropic.com/research/AI-assistance-coding-skills)

**The illusion of understanding**
Because AI-generated code is syntactically correct and superficially functional, developers often *feel* like they understand it. This is the dangerous part. The illusion holds until something breaks in a way that exceeds the developer's shallow mental model — at which point the illusion collapses into confusion and slow incident response.

**Simon Willison's bar**
> *"If you're going to put your name to it, you need to be confident that you understand how and why it works — ideally to the point that you can explain it to somebody else."*
→ [simonwillison.net/2025/Mar/6/vibe-coding](https://simonwillison.net/2025/Mar/6/vibe-coding/)

This is a practical test, not a philosophical one. If you can't pass it, you have an opacity problem.

---

## What it costs you

The consequences are slow to appear and hard to attribute — which is what makes opacity dangerous.

- **Slower incident response** — when something breaks at 2am, you're debugging code you never understood
- **Review theater** — PRs get approved with "LGTM" by reviewers who modeled the diff, not the change
- **Architectural drift** — decisions accumulate in the codebase that no one can explain or defend
- **Onboarding debt** — new team members inherit a codebase that even the authors can't fully explain
- **The oversight loop fails** — the human is kept in the loop to catch AI mistakes, but can't catch what they don't understand

---

## The fix

### The comprehension gate

Before merging AI-generated code, you should be able to explain — out loud, to another person — how it works and why this approach was chosen over alternatives.

If you can't, you don't merge it yet. You go back and interrogate it until you can.

This isn't about slowing down. It's about not shipping a liability. The 10 minutes you spend understanding the code now is the 3 hours you don't spend debugging it at 2am.

### Generation-then-comprehension

When using an agent, don't just accept the output. After it generates, ask it follow-up questions:
- *Why did you choose this approach over X?*
- *What would break if I changed Y?*
- *Walk me through what happens when Z fails.*

Developers who used this pattern in the Anthropic RCT scored **86% on comprehension tests** — outperforming even the manual hand-coding group (67%).

### Author the hard parts

For security-critical, money-handling, or concurrency code: write it yourself. Use AI to review your work, not to generate it. The generation effect matters most where the stakes are highest.

### Add this to your AGENTS.md

```markdown
## Comprehension requirements

- Never generate code without explaining WHY this approach was chosen over alternatives.
- For any function longer than 20 lines, include a plain-English summary of its
  contract: inputs → outputs → side effects.
- When modifying existing code, explain what the previous code did and why the
  new version is better.

## Explanation format

After every non-trivial code block, append:
> **What this does:** [one sentence]
> **Why this approach:** [one sentence]
> **What could go wrong:** [one sentence]
```

---

## The connection to the other failure modes

Opacity is the first link in the causal chain. You can't be accountable for code you don't understand. You over-trust the tool to compensate for what you can't verify. The whole loop starts here.

Fixing comprehension is upstream of everything else.

→ Next: [Trust Miscalibration](./trust-miscalibration.md)

---

## Sources

- Anthropic (2025). AI Assistance and Coding Skills RCT. [anthropic.com/research/AI-assistance-coding-skills](https://www.anthropic.com/research/AI-assistance-coding-skills)
- Slamecka, N.J. & Graf, P. (1978). The generation effect: Delineation of a phenomenon. *Journal of Experimental Psychology: Human Learning and Memory*, 4(6), 592–604.
- Willison, S. (2025). Vibe coding is not an excuse. [simonwillison.net/2025/Mar/6/vibe-coding](https://simonwillison.net/2025/Mar/6/vibe-coding/)
- dhung.dev (2025). AI Wrote My Code. I Couldn't FEEL It. [dhung.dev/blog/ai-wrote-my-code-i-couldnt-feel-it](https://dhung.dev/blog/ai-wrote-my-code-i-couldnt-feel-it)
