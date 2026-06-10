---
URL: https://mlflow.org/prompt-registry  |  https://mlflow.org/docs/latest/genai/prompt-registry/
Title: Prompt Registry for LLMs & Agents | MLflow Agent Platform (+ official docs)
AccessDate: 2026-06-10
Related section: KPI — registry coverage, versioning, audit trail; Roadmap — tooling
---

## Excerpt

MLflow Prompt Registry is the open-source reference implementation for prompt asset management.

### Core capabilities
- **Version control**: Git-inspired immutable versioning. Every change creates a new version number; template text cannot be mutated after creation.
- **Commit messages**: Each version carries author, timestamp, and commit message — forming an audit trail.
- **Aliases**: Mutable pointers (e.g., "production", "staging") decouple deployment from version numbers. Applications load `prompts:/qa-assistant/production`; alias promotion requires no code redeploy.
- **Lineage**: Integrated with MLflow Tracing, Evaluation, and Monitoring for end-to-end observability.
- **Model config**: Stores temperature, max_tokens, model_name alongside each prompt version.
- **Search/filter**: Tag-based discovery (`task='summarization'`) across the registry.
- **Caching**: Version-based prompts cached with infinite TTL (immutable); alias-based prompts default 60-second TTL.

### Key use cases cited
- A/B testing prompt variants via eval comparison before promoting to production
- Multi-environment deployment: dev → staging → production alias flow
- Domain expert collaboration: non-engineers edit via UI, engineers govern via CODEOWNERS
- Compliance & audit trails: regulated industries track what prompts were live when

### Prompt Management definition (MLflow docs)
"The discipline of organizing, versioning, testing, and deploying prompts across an organization's AI applications."

### Core problems solved (from MLflow docs)
1. Slow iteration: prompts hardcoded in app code require full deploy for every text change
2. No quality gates: changes go to production without testing → regressions
3. Team bottlenecks: only engineers can update, blocking domain experts
4. No reproducibility: can't debug regressions without version history
