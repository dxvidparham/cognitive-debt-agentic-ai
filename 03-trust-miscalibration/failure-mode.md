# Trust Miscalibration — Your confidence in AI output exceeds its actual reliability

> *"Developers forecast a 24% speedup. They still estimated a 20% speedup afterward. They were actually 19% slower."*
>
> — METR Research, controlled trial, 2025

---

## What it is

Trust miscalibration is when your confidence in AI output doesn't match its actual reliability.

It almost always runs in one direction: **over-trust**. You accept the output because it looks right, reads confidently, and passes a quick scan. The AI never says "I'm not sure about this." It just generates — fluently, plausibly, and sometimes incorrectly.

The dangerous part isn't that AI makes mistakes. It's that it makes mistakes in a way that's hard to detect. The code compiles. The tests pass. The logic is subtly wrong.

---

## The evidence

**METR RCT (2025) — the perception gap**

METR Research ran a controlled trial with 16 experienced open-source developers working on 246 real GitHub issues. These weren't juniors — they were contributors to major OSS projects.

Result: AI assistance made them **19% slower**.

Before the study, they forecast a 24% speedup. After the study — having just experienced being slower — they still estimated they'd been 20% faster.

The perception gap didn't close even when confronted with the reality. That's not a measurement error. That's a structural feature of how AI-assisted work *feels* versus what it *produces*.
→ [metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)

**Stanford security study (2023)**

47 developers were given 5 security-sensitive coding tasks. Half used AI assistance.

The AI-assisted group wrote significantly less secure code on 4 out of 5 tasks. But here's the part that matters: they were **more likely to believe their code was secure** than the group that wrote it by hand.

More confident. Less correct. That's trust miscalibration in its purest form.
→ [arXiv:2211.03622](https://arxiv.org/abs/2211.03622)

**Stack Overflow 2025 — the adoption-trust gap**

- **84%** of developers use AI tools
- Only **3%** highly trust AI output accuracy
- **46%** actively distrust it
- Accuracy trust *fell* from 40% to 29% year-over-year — while adoption *rose*

Developers are using tools they don't trust, at scale, every day. That gap between adoption and trust is where miscalibration lives.
→ [survey.stackoverflow.co/2025](https://survey.stackoverflow.co/2025)

**Security compounds over iterations**

A systematic analysis of iterative AI code generation found that security vulnerabilities climb from an average of **2.1 per sample** in early iterations to **6.2 by iterations 8–10** — a **195% increase** in vulnerabilities after just a few refinement cycles.

The more you iterate with an unverified agent, the worse the security posture gets — even as the code looks increasingly polished.
→ [researchgate.net — Security Degradation in Iterative AI Code Generation](https://www.researchgate.net/publication/398468388_Security_Degradation_in_Iterative_AI_Code_Generation_A_Systematic_Analysis_of_the_Paradox)

---

## Why it's hard to catch

AI tools are fluent. Fluency is a powerful social signal — we associate confident, well-structured communication with correctness. It's a reasonable heuristic for humans. It doesn't apply to language models.

The model generates the same confident tone whether it's right or wrong. There's no hesitation, no "I'm not sure about this edge case." The output looks authoritative regardless of its accuracy.

Automation researchers call this **automation bias** — the tendency to over-rely on automated systems, especially when they present output confidently. Parasuraman & Manzey (2010) showed this affects experts and novices equally, and **cannot be corrected by training or instructions alone**. The bias is structural, not a knowledge gap.

---

## What it costs you

- Accepting subtly wrong code because it looked right
- Under-testing because "the agent seems confident"
- Security vulnerabilities compounding across iterative refinement cycles
- Teams that *feel* productive but aren't — the METR finding at team scale
- Decisions made on the feeling of speed rather than measured reality

---

## The fix

### Distrust fluency by default

Treat confident AI output as *unverified*, not *correct*. The tone tells you nothing about the accuracy. Build the habit of asking "what would have to be true for this to be wrong?" before accepting any non-trivial output.

### Measure, don't feel

Your subjective sense of how fast you're working is unreliable. METR proved this with experienced developers on real tasks. The fix is external measurement: track actual cycle time, defect rates, and time-to-merge — not how productive the session felt.

If you're not measuring, you're flying on the feeling. And the feeling is wrong.

### Security gets no benefit of the doubt

For any code touching authentication, authorization, encryption, input validation, or database schemas: treat AI output as guilty until proven innocent. Review it independently. Don't accept "it looks right" as a bar.

### Add this to your AGENTS.md

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

## The connection to the other failure modes

Trust miscalibration sits in the middle of the causal chain. It flows from Opacity — you over-trust the tool partly to compensate for code you don't fully understand. And it feeds into Context-Switching — over-trust means accepting large diffs you haven't modelled, which increases the supervision burden on every subsequent review.

→ Back: [Accountability Gap](../02-accountability-gap/failure-mode.md) · Next: [Context Switching](../04-context-switching/failure-mode.md) · Mitigations: [mitigation.md](./mitigation.md)

---

## Sources

- METR Research (2025). Early 2025 AI Experienced OS Dev Study. [metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)
- Perry, N., Srivastava, M., Kumar, D., & Boneh, D. (2023). Do Users Write More Insecure Code with AI Assistants? [arXiv:2211.03622](https://arxiv.org/abs/2211.03622)
- Stack Overflow (2025). Developer Survey 2025. [survey.stackoverflow.co/2025](https://survey.stackoverflow.co/2025)
- Parasuraman, R. & Manzey, D.H. (2010). Complacency and Bias in Human Use of Automation. *Human Factors*, 52(3), 381–410.
- Tihanyi, N., et al. (2024). Security Degradation in Iterative AI Code Generation. [researchgate.net](https://www.researchgate.net/publication/398468388_Security_Degradation_in_Iterative_AI_Code_Generation_A_Systematic_Analysis_of_the_Paradox)
