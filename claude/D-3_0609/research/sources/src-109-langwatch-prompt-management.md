# src-109: LangWatch – What is Prompt Management?

**URL:** https://langwatch.ai/blog/what-is-prompt-management-and-how-to-version-control-deploy-prompts-in-productions  
**Title:** What is Prompt Management? And how to version, control & deploy prompts in productions  
**AccessDate:** 2026-06-09  
**Related section:** Production Prompt Definition / Versioning / Collaboration / Deployment

---

## Relevant Excerpt

### What Makes Up a Production Prompt
A production prompt is not just text. It includes multiple components that must be managed together:
- Instructions defining the task
- Context and system messages
- Variables for dynamic inputs
- Model parameters (temperature, max tokens, tools)
- Output constraints and guardrails

LangWatch treats all of these as a **single versioned unit**, ensuring changes are tracked, reviewed, and deployed consistently.

### Immutable Versions
Every saved prompt receives a **unique, immutable version ID**. Once created, it never changes. Any edit produces a new version. This guarantees that loading a specific version always returns the same prompt text, parameters, and metadata — making production behavior reproducible and debuggable.

### Environment Separation
Prompt versions move through development, staging, and production as explicit steps. Each environment runs its own active version, and promotion only happens after validation.

### CLI-Based Version Control (Prompts as Code)
```bash
langwatch prompt init
langwatch prompt add agents/support
langwatch prompt sync
```
Store prompts as YAML in Git; sync local changes to the LangWatch registry; pin prompt versions in production.

### Reference Architecture
1. Prompt registry
2. Evaluation engine
3. Deployment controller
4. Observability and tracing
5. Rollback controls
6. Feedback loops

### Problem Framing
"As LLM applications and AI agents mature, prompts are no longer experiments, they are production dependencies."
"Prompt management is what allows prompts to change frequently without introducing risk."

### A/B Testing Integration with Feature Flags
Reference to integration with FlagSmith for feature flags: routing live traffic across prompt versions and measuring quality outcomes before rolling anything out broadly.
