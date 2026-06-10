---
URL: https://www.zenml.io/blog/prompt-engineering-management-in-production-practical-lessons-from-the-llmops-database
Title: Prompt Engineering & Management in Production: Practical Lessons from the LLMOps Database — ZenML Blog
AccessDate: 2026-06-10
Related section: KPI — reuse rate, prompt harness packaging; Roadmap — real-world company examples
---

## Excerpt

Published: December 11, 2024. Author: Alex Strick van Linschoten.
Based on ZenML LLMOps Database (1,711+ real production implementations).

### Company case studies

**Weights & Biases (Wandbot)**
- Versioned prompt repository + comprehensive evaluation dataset → systematic prompt testing
- Automated evaluation framework with expert-annotated benchmark datasets
- "By treating prompts as versioned artifacts, with clear lineage and performance metrics, teams can more effectively manage the prompt development lifecycle."

**Canva (incident review system)**
- Structured prompts with explicit task instructions, format, and examples
- Automated scoring systems for systematic evaluation at scale

**Assembled (test generation at scale)**
- Techniques: reducing low-information context, concise instructions, modularizing prompts for specific tasks
- Result: improved productivity, reduced test-writing overhead, scalable LLM-driven workflows
- **Reuse metric implied**: "consult prompt library before crafting from scratch for new use case"

**Fiddler (documentation chatbot)**
- Iterative refinement using RAG; domain-specific prompt templates
- Prompt reuse enables adaptation across diverse user queries without re-engineering from scratch

**Ubisoft (game content)**
- Human-in-the-loop review: scriptwriters in editing/approval flow
- Output-based token pricing model with AI21 Labs (aligning prompting costs with value creation)

**Instacart**
- CoT prompting embedded in CI-style development toolchain
- Internal tool "Ava" + Monte Carlo for prompt engineering integration

**Microsoft (cloud incident management)**
- Error analysis to identify failure modes, systematic prompt updates
- Continuous improvement loop driven by production error data

### Key challenges ahead (from ZenML)
"Current versioning systems and evaluation frameworks need to evolve to handle prompt drift, regression testing, and systematic performance tracking across model versions."

### Harness reuse insight
"Maintaining a prompt library that documents proven patterns for common tasks allows engineers to consult this library before crafting prompts from scratch when facing a new use case."
