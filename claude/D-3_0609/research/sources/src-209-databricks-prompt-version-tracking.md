# src-209 — Track Prompt Versions Alongside Application Versions (Databricks/MLflow)

**URL:** https://learn.microsoft.com/en-us/azure/databricks/mlflow3/genai/prompt-version-mgmt/prompt-registry/track-prompts-app-versions
**Title:** Track prompt versions alongside application versions — Azure Databricks
**Author:** Microsoft / Databricks
**AccessDate:** 2026-06-09
**Related section:** KPI (Dependency Map Coverage, Reproducibility), Roadmap

## Key Excerpts (from Search Result Snippet)

> "MLflow 3.0 extended its model registry to handle generative AI applications and AI agents, connecting models to exact code versions, prompt configurations, evaluation runs, and deployment metadata."

> "MLflow automatically creates **lineage between your prompt versions and application versions**. MLflow creates a LoggedModel that serves as a metadata hub for your application version, acting as a central record that links to your external code (like a Git commit), configuration parameters, and automatically tracks which prompts from the registry your application uses."

> Prompt versioning in MLflow: "each version can be tagged with aliases like 'production' or 'staging,' allowing teams to promote tested prompt versions through environments without redeploying application code."

## Note
This confirms that dependency/lineage mapping between prompt assets and the downstream application versions is technically achievable and implemented in production-grade platforms (MLflow 3.0 / Azure Databricks). This directly supports the "Dependency-Map Coverage" KPI.
