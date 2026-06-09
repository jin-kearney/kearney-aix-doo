# src-202 — MLflow Prompt Registry

**URL:** https://mlflow.org/docs/latest/genai/prompt-registry/
**Title:** Prompt Registry | MLflow AI Platform
**Author:** MLflow Project (LF Projects, LLC)
**AccessDate:** 2026-06-09
**Related section:** KPI (Version-Control Coverage, Metadata Schema, Dependency Map/Lineage), Roadmap (registry implementation)

## Key Excerpts

> "MLflow Prompt Registry is a powerful tool that streamlines prompt engineering and management in your LLM applications and AI agents. It enables you to version, track, and reuse prompts across your organization, helping maintain consistency and improving collaboration in prompt development."

> Key attributes of a Prompt object: `Name`, `Template`, `Version` (sequential number), `Commit Message` (description of changes, similar to Git commit messages), `Tags` (key-value pairs for categorization), `Alias` (mutable named reference to a specific version, e.g., 'production', 'staging').

> "Prompt versions in MLflow are immutable, providing strong guarantees for reproducibility."

> "When you use mlflow.set_active_model() with prompts from the registry, MLflow automatically creates **lineage between your prompt versions and application versions**. MLflow creates a LoggedModel that serves as a metadata hub for your application version... and automatically tracks which prompts from the registry your application uses."

> Tags example: `"author": "author@example.com"`, `"task": "summarization"`, `"language": "en"` — illustrating standard metadata fields.

> `model_config` field stores model-specific configuration alongside prompts, ensuring reproducibility: model_name, temperature, max_tokens, top_p, stop_sequences, etc.

> "Version-based prompts (e.g., prompts:/summarization-prompt/1): Cached with infinite TTL by default. Since prompt versions are immutable, they can be safely cached indefinitely." — supports reproducibility KPI.

> "Build robust yet flexible deployment pipelines for prompts, allowing you to isolate prompt versions from main application code and perform tasks such as A/B testing and roll-backs with ease."
