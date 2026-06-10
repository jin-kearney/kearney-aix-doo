# src-303 — Prompt Rot: Why Your Carefully Tuned Prompts Break When You Change LLMs

- **URL:** https://medium.com/@yadavvaibhav0104/prompt-rot-why-your-perfect-prompts-stop-working-and-how-to-fix-them-3d45dd79cc60
- **Title:** Prompt Rot: Why Your Carefully Tuned Prompts Break When You Change LLMs
- **AccessDate:** 2026-06-10
- **Related section:** Why (dependency/fragility argument — prompt rot concept)

---

## Key Excerpts

> "Swapping one Large Language Model for another often looks trivial: change the API endpoint, rerun tests, ship. In practice, many teams discover a quieter failure mode — prompts that once worked perfectly now behave *just differently enough* to break assumptions. No errors. No crashes. Just subtle drift."

### What Is Prompt Rot?

> "Prompt Rot happens when prompts — especially those with few-shot examples, rigid phrasing, or 'magic' wording — are implicitly overfit to a specific model version. When the model changes, those prompts may: ignore examples they once followed; drift in tone or verbosity; violate output formats; reinterpret instructions."

### Prompts Are More Than Instructions

> "Complex prompts act like **tiny models**. By adding examples, repeated phrasing, and carefully ordered text, we shape the model's probability distribution — not just its understanding. This works until a newer model: uses different tokenization; has updated alignment behavior; follows instructions more literally; avoids brittle pattern matching. What looked like 'reliable prompting' was often an undocumented interaction with the old model."

### Prompt Engineering as a Hypothesis

> "Prompts are not contracts. They are **hypotheses about how a specific model will interpret instructions**. When the model changes, the hypothesis must be revalidated."

### Why Prompt Rot Is Dangerous

> "Prompt Rot rarely causes hard failures. Instead, it shows up as: unexplained verbosity; new disclaimers; inconsistent formatting; edge-case hallucinations. Because responses still 'look reasonable,' these issues are easy to misattribute to randomness or temperature changes."

### Final Thought

> "Prompt Rot doesn't mean prompt engineering is bad. It means prompts are **powerful, fragile, and tightly coupled to models** — whether we acknowledge it or not. If models are evolving, prompts must be treated as living artifacts too."

---

*Author: Vaibhav Yadav. Published Jan 27, 2026.*
