---
URL: https://docs.datadoghq.com/llm_observability/monitoring/prompt_tracking/
Title: Prompt Tracking — Datadog Agent Observability Documentation
AccessDate: 2026-06-10
Related section: KPI — audit trail, version-linked metrics, production tooling evidence
---

## Excerpt

Fetched directly from Datadog official documentation (generally available feature in Agent Observability / LLM Observability).

### Core definition
"Prompt Tracking is a feature that links prompt templates and versions to LLM calls. Prompt Tracking works alongside Agent Observability's traces, spans, and Playground."

### What it enables (production KPI tooling)
- **Prompt call count**: Timeseries chart — calls per prompt (or per version) over time
- **Call volume and latency over time**: Per-prompt, per-version trending
- **Version comparison**: Compare prompts or versions by calls, latency, tokens used, and cost
- **Version history**: Side-panel view showing version activity with diff (text diff) between versions
- **Audit trace**: Each trace shows prompt template + variables + full telemetry (latency, token count, eval metrics)
- **Filter by prompt name, ID, or version**: Isolate impacted requests in Trace Explorer
- **Playground reproduction**: Reproduce any span using the exact template + variables from any historic version

### Prompt metadata schema (official fields)
| Field | Required | Description |
|-------|----------|-------------|
| `template` | Yes (or `chat_template`) | Template string for single-message prompts |
| `chat_template` | Yes (or `template`) | List of `{role, content}` message templates |
| `id` | No | Unique prompt identifier |
| `name` | No | Human-readable prompt name |
| `version` | No | User-supplied version tag |
| `variables` | No | Template variable substitutions |
| `rag_context_variables` | No | Variables containing RAG context (ground truth for evaluators) |
| `rag_query_variables` | No | Variables containing user query (for RAG evaluators) |

### Key quote
"When a tracked prompt is called, LLM Observability automatically attaches the corresponding version metadata to your LLM spans and traces, giving you an immediate, auditable link between each prompt change, its deployment, and the observed performance."

### Implication for D-3 KPIs
Datadog's implementation provides the tooling layer for: audit-trail completeness (every production call linked to prompt@version), regression detection (latency and eval metric trends per version), and incident investigation (filter traces by version to isolate regressions). This is a primary-source confirmation that these KPIs are operationally achievable in enterprise tooling.

### LangChain auto-instrumentation
"If you are using LangChain prompt templates, Datadog automatically captures prompt metadata without code changes." — confirms industry-standard tooling supports these KPIs without custom instrumentation.
