# src-106: NeoSage / Dimple Sharma – The Prompt Lifecycle Every AI Engineer Should Know

**URL:** https://blog.neosage.io/p/the-prompt-lifecycle-every-ai-engineer  
**Title:** The Prompt Engineering Playbook (The Prompt Lifecycle Every AI Engineer Should Know)  
**AccessDate:** 2026-06-09  
**Related section:** Prompt Asset Lifecycle / YAML Schema / Design Pattern / Registry Concept

---

## Relevant Excerpt

### The Prompt Lifecycle (5 Stages)
1. **Design** – define the prompt as a structured, version-controlled asset; use templates to separate logic from data; define schema with metadata, parameters, and target model
2. **Test** – multi-layered evaluation (golden datasets → metric-based → runtime verification)
3. **Deploy** – publish to a centralized Prompt Registry; applications fetch specific versioned prompts dynamically
4. **Monitor** – collect real-world telemetry: latency, token costs, quality scores, user feedback
5. **Maintain** – version, improve, or retire based on monitoring data

### YAML Schema for a Structured Prompt Asset
```yaml
# summarize_ticket.v1.yaml
name: SummarizeSupportTicket
description: "Generates a concise summary of a user support ticket for internal review."
version: 1
tags: ['support', 'summarization']
input_variables:
  - support_ticket_text
  - output_format  # Can be "detailed" or "summary"
execution_settings:
  model: "claude-3-opus-20240229"
  temperature: 0.5
  max_tokens: 512
template: |
  Your task is to extract information from the following support ticket.
  Support Ticket: {{ support_ticket_text }}
  {% if output_format == "detailed" %}
  Please format the output as a detailed JSON object...
  {% endif %}
```

This YAML file becomes "the single source of truth for your prompt's structure and requirements" — a formal specification with a name, version, explicit inputs, and the exact model settings it was tested against.

### Anti-Pattern Identified
Treating prompts as hardcoded strings is called "Prompt Rot": behavior becomes tightly coupled to a specific model version; silent regression occurs without any code change.

### Key Concept: Versioning in Git
YAML files are committed to Git: provides a complete audit trail (git blame), safe rollbacks (git revert), and clear comparisons (git diff).

### Evaluation Maturity Model
- **Level 1:** Curated golden datasets (canonical benchmark; representative inputs + ideal outputs)
- **Level 2:** Automated metric-based evaluation (deterministic exact match + semantic similarity + LLM-as-judge)
- **Level 3:** Runtime verification (Pydantic schema enforcement via Instructor library)

### Production Problems Solved
| Problem | Root Cause | Solution |
|---|---|---|
| "Prompt Rot" | Coupled to model version, silent regression | Versioning + structured schema |
| "Versioning Black Hole" | No single source of truth | Centralized registry with version IDs |
| "Observability Black Box" | No telemetry on prompt strings | OpenTelemetry instrumentation |
| "Economic Drain" | Bloated/inefficient prompts | Cost tracking per version |
| "Security Blindspots" | Unvalidated user input to LLM | Structured output validation |
