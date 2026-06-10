# src-202 — Nearform: Prompt Management Systems Compared

**URL:** https://nearform.com/digital-community/prompt-management-systems-compared/  
**Title:** Prompt Management Systems Compared  
**AccessDate:** 2026-06-10  
**Related section:** How-3 (Version Management), How-8 (Tool Map)

---

## Relevant Excerpt

At Nearform, managing prompts became more difficult — prompts were hardcoded into application code, and similar prompts spread across different codebases. With prompt management systems:

- Prompts are now managed independently of application code, allowing teams to update and refine them quickly without redeploying the entire application.
- Shared prompt registries make it easy for teams across the organization to collaborate on and reuse prompts.

**Evaluation criteria used:**
1. Prompt versioning & registry — ability to track history of a prompt through different iterations
2. Team collaboration — ability for team members to share prompts
3. Prompt composability — ability to compose prompts by combining reusable snippets
4. Prompt evaluation — ability to evaluate prompts based on how well it guides the system to produce the desired response

**Recommended tools:**
- **Langfuse** — open-source LLM engineering platform; most full-featured; MIT license; self-hostable
- **MLflow** — Linux Foundation project; auto versioning; Databricks-managed version has more features

**Tools that didn't make the cut and why:**
- **Prompt flow (Microsoft)** — cannot version individual prompts, only entire LLM applications; vendor lock-in to OpenAI/Azure
- **LangSmith** — closed-source, managed service only; vendor lock-in
- **Braintrust** — closed-source, does not fully support self-hosting
- **PromptLayer** — self-hosting requires Enterprise license
- **Agenta** — prompts not first-class citizens; evaluation features only in paid version
- **Helicone** — primarily an AI gateway; lacks ability to run evaluations
- **Humanloop** — acquired by Anthropic in August 2025; platform sunset September 8, 2025

**Key finding on prompt sprawl:**
"Prompts were hardcoded into application code, and we often saw similar prompts spread across different codebases."

**Comparison table (Langfuse vs MLflow):**
| Feature | Langfuse | MLflow |
|---|---|---|
| GitHub Stars | 16.9k | 22.4k |
| License | MIT Expat | Apache-2.0 |
| Backers | Y Combinator | Linux Foundation, Databricks |
| Self-hosting | Easy (official Terraform/Helm) | No dedicated guide |
| Prompt composability | Yes (embed prompts via special syntax) | Variables only |
| RBAC | 5 predefined roles | Experimental (as of v3.4.0) |
| Open-source completeness | Most features in OSS | Many features behind managed version |
