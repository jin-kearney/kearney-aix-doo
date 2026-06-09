# src-105: MLflow – Prompt Registry Documentation

**URL:** https://mlflow.org/docs/latest/genai/prompt-registry/  
**Title:** Prompt Registry | MLflow AI Platform  
**AccessDate:** 2026-06-09  
**Related section:** Prompt Asset Schema / Model Config / Tags / Aliases / Governance

---

## Relevant Excerpt

### Prompt Object Key Attributes
- `Name` – unique identifier for the prompt
- `Template` – content of the prompt; either:
  - A string with variables in `{{variable}}` format (text prompts)
  - A list of dicts with 'role' and 'content' keys (chat prompts)
- `Version` – sequential number representing the revision (immutable)
- `Commit Message` – description of changes made (similar to Git commit messages)
- `Tags` – optional key-value pairs for categorization and filtering (e.g., project name, language, author, task type)
- `Alias` – mutable named reference (e.g., `production`, `staging`, `latest`)
- `response_format` – optional: expected response structure specification (Pydantic model or JSON schema dict)
- `model_config` – optional: model-specific configuration (model_name, temperature, max_tokens, top_p, frequency_penalty, presence_penalty, stop_sequences, extra_params)

### Model Configuration Fields (PromptModelConfig)
```python
config = PromptModelConfig(
    model_name="gpt-4-turbo",
    temperature=0.5,
    max_tokens=2000,
    top_p=0.95,
    frequency_penalty=0.2,
    presence_penalty=0.1,
    stop_sequences=["END", "\n\n"],
)
```

### Prompt Registration with Tags
```python
prompt = mlflow.genai.register_prompt(
    name="summarization-prompt",
    template=initial_template,
    commit_message="Initial commit",
    tags={
        "author": "author@example.com",
        "task": "summarization",
        "language": "en",
    },
)
```

### Alias System
Mutable references (e.g., "production", "staging") for A/B testing and gradual rollouts without changing application code. Single alias update = instant promotion.

### Versioning Principles
- Prompt versions are **immutable** once created (strong reproducibility guarantees)
- Git-inspired commit-based versioning with side-by-side diff highlighting
- Infinite TTL caching for version-based lookups; 60-second TTL for alias-based lookups

### Governance (Databricks/Unity Catalog Integration)
- Access control and audit trails via Unity Catalog
- Track lineage by linking prompts to experiments and evaluation results
- "Regulated industries can track what prompts were used when and by whom"
- Complete audit trail of every version change, who made it, and which environments it was deployed to

### Search Capability
```python
prompts = mlflow.genai.search_prompts(filter_string="task='summarization'")
```
Search by name, tag, or other registry fields.

### Key Benefits Summary
1. **Version Control** – Git-inspired commit-based versioning with diff highlighting
2. **Aliasing** – Flexible deployment pipelines with A/B testing and rollbacks
3. **Lineage** – Integrated with MLflow Tracing, Evaluation, and Monitoring
4. **Collaboration** – Centralized registry for sharing across the organization
