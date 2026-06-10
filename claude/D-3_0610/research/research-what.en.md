# D-3 Prompt/Harness Asset-ization: What (Core) Research Notes

**Topic:** D-3 — Prompt/Harness Asset-ization  
**Cluster:** What (core) — definition / components / structure / standards & practices / glossary  
**Author/Date:** Research compiled 2026-06-10  

---

## 1. What Exactly Is a "Prompt Asset"?

### 1.1 Plain Definition

A **prompt asset** is a prompt that has been elevated from a disposable, hardcoded string into a **managed, versioned, reusable artifact** — with a formal schema, version history, ownership metadata, and a place in a central registry.

The shift in mindset is captured by a practitioner quote: "Treat every hardcoded prompt as a high-severity bug." [src-108]

**Simple analogy:**  
A prompt asset is to a raw AI instruction what an **SOP document in a quality management system** is to a casual verbal instruction. The SOP has a document number, version, author, approval record, review date, and controlled distribution. The casual verbal instruction has none of these. Both "work" — but only the SOP scales, audits, and improves reliably.

A prompt registry is, accordingly, analogous to an **SOP document library with version control** — a centralized repository where every approved work procedure lives in a governed, findable, retrievable form.

### 1.2 The "Prompt as Data, Not Code Snippet" Perspective

The industry has converged on treating prompts as **first-class data assets** rather than implementation details embedded in code:

> "The future of AI engineering is building systems that manage prompts, not perfecting individual prompts." [src-108]

> "In 2025, prompt management is no longer optional — it's infrastructure." [src-105 context]

Evidence that this is now infrastructure thinking (not just tooling preference):
- **Amazon Bedrock Prompt Management** (AWS) — natively built into the cloud platform [src-111]
- **Azure AI Foundry Prompt Flow** — prompt assets are versioned flow artifacts in the Azure AI ecosystem
- **LangSmith Prompt Hub** — commit-based versioning with environment promotion (Staging → Production) [src-103]
- **Langfuse** (open-source, self-hostable) — prompt management as a core platform feature alongside observability [src-106]
- **Humanloop** (acquired by Anthropic, Aug 2025) — prompt management focused on collaborative iteration with non-engineers [src-105]

The organizational case: When 500 employees each use their own version of the same prompt, the result is 500 different outputs. A customer support prompt containing incorrect legal language embedded into every AI-assisted contract draft is not an engineering problem — it is a governance failure. [src-107]

---

## 2. What Is a "Harness"?

### 2.1 Authoritative Definition

The cleanest and most widely cited definition comes from LangChain (Vivek Trivedy, Mar 2026):

> **"A harness is every piece of code, configuration, and execution logic that isn't the model itself."**  
> **"Agent = Model + Harness. If you're not the model, you're the harness."** [src-101]

MongoDB (Bazeley, Kumar, Xu, Apr 2026) expands this:

> "The harness comprises six components around the model. The layer underneath them is what turns a harness into a platform." [src-102]

### 2.2 Types of Harness in the LLM Ops World

The term "harness" appears in three related but distinct contexts. For D-3, it is important to be precise:

| Harness Type | What It Is | D-3 Relevance |
|---|---|---|
| **Agent harness** | The full execution environment around an LLM agent (system prompts + tools + memory + orchestration + sandboxes) | High — the system prompt, role definition, tool descriptions, and orchestration logic ARE the harness; structuring these as managed assets is D-3 |
| **Task harness** | A specific bundle for a repeatable task: (prompt + input data wiring + tool-call sequence + output contract + checks) | High — this is what D-3 calls "harness as work unit" |
| **Evaluation harness** | Infrastructure for systematically running test cases against prompts/agents (belongs to E-3/E-4 boundary) | Out of D-3 scope — evaluation criteria and gold datasets belong to E-3 |

### 2.3 The Six Harness Components (MongoDB Taxonomy)

1. **State and persistence** — stores execution context, checkpoints, agent's position within multi-step task; determines whether agent resumes or restarts after crash [src-102]
2. **Security and governance** — identity propagation, permission scoping, auditability; "where most enterprise agent projects stall before production" [src-102]
3. **Orchestration and tool use** — drives the agent loop (ReAct: reason → action via tool call → observe → repeat); tool calls to external systems (APIs, databases, search, other agents) [src-101, src-102]
4. **Memory** — two levels: agent-side working memory (context window contents) and harness-side persistent store (episodic, conversation, entity stores via vector index) [src-102]
5. **Observability** — records decisions, tool calls, and outcomes; "fundamentally different from application monitoring: the question is not whether the system responded but whether the decision it made was correct" [src-102]
6. **Evals** — run out-of-band from observability data; determine whether agent is ready to deploy and whether it should remain deployed [src-102]

**What belongs in D-3 vs. neighboring topics:**
- Harness components 1–4 (state, governance, orchestration, memory) → **D-3** owns the prompt/instruction elements: system prompts, role definitions, judgment criteria, tool descriptions, orchestration logic
- Component 5 (observability) → **D-3** owns prompt version tracking; execution tracing belongs to E-4
- Component 6 (evals) → **E-3** (evaluation datasets, gold standards); the evaluation harness infrastructure itself belongs to E-3/E-4
- Tool specs (what tools do, their APIs, schemas) → **D-2**

### 2.4 Harness vs. Agent Framework

**Harness ≠ Agent framework.** The distinction matters:

- An **agent framework** (LangChain, LangGraph, CrewAI, AutoGen) provides reusable software abstractions for building agents
- A **harness** is the specific configuration and execution environment for a particular agent task — it IS an asset, configurable per use case
- The same framework can run many different harnesses; the harness encodes the judgment and procedure for a specific task

Supporting evidence: LangChain's Terminal Bench 2.0 result showed Opus 4.6 in Claude Code far below Opus 4.6 in other harnesses. By changing ONLY the harness (not the model, not the framework), the coding agent moved from Top 30 to Top 5. [src-102]

### 2.5 Harness as Encoded Organizational Procedure

The Anthropic example is the clearest illustration of harnesses as organizational data assets:

Anthropic's two-agent harness for long-running coding agents consists of:
1. **Initializer agent** — a structured prompt that instructs the first session to set up: an `init.sh` script, a `claude-progress.txt` file (log of work done), and an initial git commit
2. **Coding agent** — a structured prompt that instructs each subsequent session to: read git logs + progress file → choose one feature → implement incrementally → write git commit + progress update → leave in clean state [src-109]

> "The key insight here was finding a way for agents to quickly understand the state of work when starting with a fresh context window... Inspiration for these practices came from knowing what effective software engineers do every day." [src-109]

**Manufacturing analogy:** This is exactly what a shift changeover SOP does in a production plant: the outgoing shift operator documents machine state, work done, anomalies, and next priorities so the incoming operator can start without context loss. The harness encodes this procedure as AI-executable instructions.

---

## 3. Components of a Managed Prompt Asset

### 3.1 The Formal YAML Schema (from NeoSage / Dimple Sharma, Feb 2026)

The following is the canonical example of a prompt defined as a structured data asset [src-108]:

```yaml
# summarize_ticket.v1.yaml
# METADATA
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

# TEMPLATE (Jinja2 for variable injection)
template: |
  Your task is to extract information from the following support ticket.
  Support Ticket: {{ support_ticket_text }}
  
  {% if output_format == "detailed" %}
  Please format the output as a detailed JSON object with keys: 
  'ticketId', 'customerEmail', 'submittedAt', 'productTier', 'fullIssueDescription'
  {% elif output_format == "summary" %}
  Please format the output as a compact JSON object with only: 
  'ticketId' and 'issueSummary'
  {% endif %}
```

### 3.2 Complete Field List for a Managed Prompt Record

Drawing from multiple sources [src-103, src-104, src-108, src-111, src-107], a fully managed prompt asset record includes:

**Identification & Versioning:**
- `name` / `id` — unique identifier
- `version` / `commit_hash` — immutable version identifier
- `created_at`, `updated_at` — timestamps
- `created_by` / `owner` — person or team responsible
- `tags` — deployment labels (staging, production) and categorical labels
- `change_reason` / `commit_message` — why this version was created

**Content Fields:**
- `system_prompt` / `template` — the core instruction text
- `role_definition` — the AI's persona or role (e.g., "You are a quality control inspector...")
- `instructions` — step-by-step task guidance
- `judgment_criteria` / `rubrics` — how to evaluate outputs (inline, not in the eval dataset)
- `few_shot_examples` — example input/output pairs embedded in the prompt
- `output_schema` / `output_format` — expected structure (JSON schema, markdown format, etc.)
- `guardrails` / `prohibited_actions` — what the AI must NOT do
- `variables` / `input_variables` — placeholder names and expected types

**Model & Execution Configuration:**
- `model` — target model ID (pinned, not an alias)
- `temperature`, `max_tokens`, `top_p` — inference parameters
- `tool_list` — which tools/MCPs this prompt is authorized to use (→ links to D-2)

**Dependency & Context Metadata:**
- `depends_on_glossary_terms` — list of business glossary terms this prompt relies on
- `depends_on_data_assets` — data tables/fields/APIs this prompt references
- `depends_on_eval_dataset` — eval dataset ID for regression testing (→ links to E-3)
- `depends_on_prompts` — other prompts upstream or downstream in a chain

**Governance:**
- `status` — draft / staging / production / deprecated
- `approved_by` — approver's name and role
- `approval_date`
- `access_level` — personal / team-scoped / org-wide
- `permitted_data_classes` — what data categories may be injected (for compliance)

### 3.3 The "Prompt as Data/Asset, Not Code-Snippet" — Three Design Principles

1. **Decouple with templates** — separate static logic from dynamic data; template engine (Jinja2) handles "how data is presented to the model"; application code only provides "what data to send" [src-108]
2. **Define a formal schema** — YAML file that bundles template + metadata; makes prompt a "specification" not a "guess"; validate with Pydantic [src-108]
3. **Establish version control** — commit to Git; provides audit trail (git blame), safe rollbacks (git revert), version comparison (git diff); "if your prompt isn't in version control, it doesn't exist" [src-108]

---

## 4. Prompt Registry / Prompt Library Concepts

### 4.1 What a Prompt Registry Is

> "A Prompt Registry is a centralized, versioned repository for your validated prompt artifacts. Instead of your application reading a local .yaml file, it fetches the prompt directly from this registry at runtime." [src-108]

> "This is a critical decoupling step. It means you can update a prompt without having to redeploy your entire application." [src-108]

**The registry as single source of truth:**
- Applications call the registry API with a prompt name + version tag at runtime
- Registry returns the correct versioned prompt
- Prompt updates deploy instantly — no code deployment cycle required
- All usage is logged (which version produced which output)

### 4.2 Environment Promotion (Staging → Production)

LangSmith's implementation [src-103] is the clearest example of software-grade deployment discipline applied to prompts:

```
Prompt version v3 → Staging tag → validate → Production tag
```

- `staging` and `production` are reserved commit tags
- Promotion UI moves a specific commit to an environment
- Rollback: revert environment pointer to previous commit
- Code references tag, not hash: `client.pull_prompt("joke-generator:production")` — changing what "production" points to requires no code change

**Webhook on commit:** Automated CI/CD pipeline can trigger on every prompt commit → run eval suite → block if quality drops → notify team. [src-103]

### 4.3 Leading Products and Their Positioning

| Solution | Vendor | Category | Key Features | Notes | Source |
|---|---|---|---|---|---|
| LangSmith Prompt Hub | LangChain | Managed SaaS + open-source | Commit-based versioning, commit tags, env promotion (Staging/Production), webhook triggers, RBAC owners, public hub for community prompts | Free (5K traces/mo); $39/mo Dev; deep LangChain integration | src-103, src-105 |
| Braintrust | Braintrust Data | Managed SaaS | Content-addressable version IDs, env-based deployment, A/B testing, tight eval integration, bidirectional PM↔engineer collaboration, CI/CD GitHub Action | Best scored (94/100); Free (1M spans); $249/mo Pro; SaaS-first | src-105 |
| Humanloop | Humanloop (acquired by Anthropic Aug 2025) | Managed SaaS | Polished UI, collaborative review workflows (assign reviewers, comments, approval), strong for non-technical teams | Best for product-led teams; $150/mo Team | src-105 |
| PromptLayer | PromptLayer | Managed SaaS | Auto-capture versioning, centralized registry, minimal integration friction, gets prompts out of application code | Best for low-friction start; $30/mo Pro | src-105 |
| Langfuse | Langfuse GmbH | Open-source + Cloud | Linear versioning, labels for environments, traces linked to prompt versions, OpenTelemetry-based observability | Self-hostable; free tier; pairs well with any LLM stack | src-106 |
| Amazon Bedrock Prompt Management | AWS | Cloud platform native | Visual prompt builder, variant comparison, variable/placeholder support, versioning, integration with Bedrock Flows | Part of AWS AI platform; region expansion ongoing | src-111 |
| Azure AI Foundry Prompt Flow | Microsoft | Cloud platform native | Visual + code-based pipeline builder, built-in versioning, deployment, variant testing, 1,800+ model catalog | Enterprise Azure ecosystem | search context |
| Agenta | Agenta AI | Open-source + Cloud | Prompt management + eval + observability; online evaluation; regression test suites from production data | Self-hostable; focuses on drift detection | src-110 |
| W&B Prompts | Weights & Biases | MLOps platform extension | Unified ML+LLM experiment tracking, comparison, team workspaces | Good for teams already using W&B for model training | src-105 |

---

## 5. Harness Structure: What Gets Bundled

### 5.1 The Task Harness as Repeatable Work Unit

A **task harness** bundles everything needed to execute a repeatable AI-mediated work procedure:

| Bundle Element | Description | D-3 Ownership |
|---|---|---|
| **System prompt + role definition** | Core instruction text; AI's persona | D-3 owns |
| **Judgment criteria / rubrics** | Inline evaluation criteria for outputs (not gold datasets) | D-3 owns |
| **Context / data wiring** | Which data assets to pull, variable schemas, RAG retrieval specs | D-3 owns the spec; D-2 handles tool; data catalog links |
| **Tool-call sequence** | Order and conditions for tool use | D-3 owns the sequence; D-2 owns tool specs |
| **Output contract / output schema** | Expected output format, JSON schema, required fields | D-3 owns |
| **Guardrails / exception handling** | What to do when inputs are out-of-scope, model refuses, errors occur | D-3 owns |
| **Eval criteria reference** | Pointer to eval dataset / test suite (not the dataset itself) | D-3 owns the reference; E-3 owns the dataset |
| **Model + inference config** | Pinned model version, temperature, max tokens | D-3 owns |
| **Dependency map** | Links to glossary terms, data assets, tools, eval datasets this harness depends on | D-3 owns |

### 5.2 Relationship to "Agent Workflow" / "AI Workflow Template" Concepts

The harness is the infrastructure that makes a workflow repeatable. From LangChain [src-101]:

> "Harnesses today are largely delivery mechanisms for good context engineering."

From Anthropic [src-109], the two-harness pattern for long-running agents shows how procedures (software engineering best practices: git commits, progress files, incremental completion, self-testing) get encoded as AI-executable harness configuration.

**Manufacturing use case examples:**

- **Quality-inspection verdict prompt**: System prompt = "You are a quality control inspector evaluating weld images against ISO 5817 standard"; Judgment criteria = rubric for defect classification; Output schema = JSON with {defect_type, severity, pass/fail, confidence}; Tool-call sequence = image analysis tool → defect database lookup → verdict generation; Guardrail = "If image quality insufficient, request re-scan rather than guessing"

- **Equipment-failure analysis prompt**: System prompt = "You are a maintenance engineer analyzing sensor data for [equipment type]"; Data wiring = variables for sensor readings, maintenance history; Judgment criteria = failure mode prioritization matrix; Output schema = structured report with root cause, confidence, recommended action; Exception handling = "If insufficient data, state data gaps explicitly"

---

## 6. Prompt Version Management

### 6.1 Core Versioning Concepts

Every leading platform implements the same core pattern, adapted from software engineering:

**Commit → Tag → Promote → Rollback**

1. **Commit**: every save creates a new immutable version with a unique hash; old versions never deleted
2. **Tag**: apply human-readable labels to commits (`v1.0_production`, `staging`, `production`)
3. **Promote**: move a tested commit from staging to production by reassigning the environment tag — no code change required
4. **Rollback**: revert environment pointer to previous commit; instantaneous

**Kore.ai naming convention examples** [src-104]:
- `v1.0_initial`
- `v2.1_refined_summarization`
- `v3.0_compliance_update`

### 6.2 What a Version Record Must Capture

For enterprise auditability [src-104, src-107]:
- Who made the change
- What changed (diff of prompt text + parameters)
- Why it changed (commit message / change reason)
- When it changed (timestamp)
- Who approved it (approver name + role)
- What environment it was deployed to and when

The regulated-industry audit question: "What prompt generated this output, who approved that prompt, and when was it last changed?" — becomes answerable only when version history is maintained. [src-107]

### 6.3 Prompt Dependency Management

A production LLM application rarely uses a single prompt in isolation. Prompts chain together — retrieval feeds generation, classification routes to specialized prompts.

**Dependency types to track:**
- **Upstream data assets**: which tables, APIs, glossary terms a prompt references
- **Downstream prompts**: prompts that consume this prompt's output
- **Tool dependencies**: which tools/MCPs this prompt calls (→ D-2 boundary)
- **Model version dependency**: which specific model version a prompt was tuned for
- **Eval dataset dependency**: which test suite validates this prompt (→ E-3 boundary)

**Why it matters for drift detection** [src-110]:
> "When you update one prompt in a chain, every downstream prompt is affected. You might improve your retrieval prompt and inadvertently change the context that your generation prompt receives. The generation prompt hasn't changed, but its outputs have."

This is a form of drift specific to multi-step systems — invisible unless dependency relationships are tracked.

---

## 7. Detecting Prompt Performance Degradation

### 7.1 Prompt Drift vs. Prompt Regression

| Term | Definition | Root Cause |
|---|---|---|
| **Prompt drift** | LLM output behavior changes over time WITHOUT any edit to the prompt | Silent model updates; input distribution shifts; dependent prompt changes |
| **Prompt regression** | A deliberate change to a prompt causes worse performance | The edit itself |

[src-110]

### 7.2 The Silent Model Update Problem

Stanford/UC Berkeley study (Chen, Zaharia, Zou, "How Is ChatGPT's Behavior Changing over Time?" 2023): GPT-4 accuracy on identifying prime numbers dropped from **84% to 51%** between March and June 2023. No version number changed. No changelog published. [src-110]

Early 2025: developers reported `gpt-4o-2024-08-06` (a supposedly fixed, dated version) had changed behavior without notification. [src-110]

**Mitigation**: Pin model versions explicitly (e.g., `gpt-4o-2024-08-06` not `gpt-4o`); build regression test suites that run before switching model versions; integrate into CI/CD.

### 7.3 Detection Framework (Three Capabilities Required)

1. **Observability**: trace every interaction linked to specific prompt version → if outputs change without prompt changes, something external shifted
2. **Automated evaluation on production traffic**: continuous LLM-as-judge or programmatic scoring; when scores drop, investigate
3. **Metrics over time**: track score drops without prompt changes, increased output variance, length creep (many model updates make responses longer), latency changes

**Alert thresholds** (Agenta pattern [src-110]): trigger alert when accuracy drops below threshold (e.g., 90%) or quality score falls by more than a set percentage (e.g., 10%).

---

## 8. MECE Distinctions: Keeping D-3 Clean

### 8.1 Prompt Engineering vs. Prompt Asset Management

| Dimension | Prompt Engineering | Prompt Asset Management (D-3) |
|---|---|---|
| Goal | Craft better instructions for a specific task | Govern, version, and operationalize prompts as organizational assets |
| Scope | One-off, individual task | Organization-wide, reusable, governed |
| Artifact | A string in code | A versioned record in a registry with metadata |
| Skill | Linguistic, iterative | Systems engineering, data governance |
| Failure mode | Bad output for one task | Silent regressions at scale; compliance failures; knowledge loss on attrition |

[src-112, src-108]

The three nested layers analogy [src-112]:
1. **Prompt engineering** — better instructions (failure: bad output for the immediate task)
2. **Context engineering** — better information architecture around the prompt (failure: stale or missing context)
3. **Harness engineering** — reliable operational environment (failure: agent can't recover from errors, persist state, coordinate)

### 8.2 D-3 vs. D-2 (Tool Specs)

- **D-3 owns**: the instruction logic telling the AI when and why to use a tool; the sequence and conditions for tool calls; guardrails around tool use
- **D-2 owns**: the tool specification itself — what the tool does, its API schema, input/output contract, authentication

In a prompt record, the `tool_list` field (what tools this prompt may use) is D-3. The tool description files themselves are D-2.

### 8.3 D-3 vs. E-3 (Evaluation Data)

- **D-3 owns**: the judgment criteria and rubrics embedded INLINE in the prompt (e.g., "evaluate outputs on a 1–5 scale for accuracy and completeness"); the pointer/reference to an eval dataset; the CI/CD pipeline that runs evals on prompt commits
- **E-3 owns**: the eval dataset itself (gold standard input/output pairs); the test suite content; human annotation guidelines

The eval harness infrastructure (running tests, scoring) belongs to E-3/E-4. The prompt-side quality gates belong to D-3.

### 8.4 D-3 vs. E-4 (Feedback Loop Operations)

- **D-3 owns**: the monitoring configuration for a prompt (what metrics to track, alert thresholds); the process of creating a new prompt version in response to detected degradation
- **E-4 owns**: the ongoing operational feedback loop — collecting user signals, routing them to improvement processes, the governance cadence for acting on feedback

---

## 9. Glossary of Must-Know Terms

| Term | Plain Definition |
|---|---|
| **System prompt** | The "standing order" given to the AI before any user interaction; defines role, constraints, output format, and behavior for a task |
| **Prompt template** | A reusable prompt text with named variable placeholders (e.g., `{{customer_name}}`); separates stable logic from dynamic data |
| **Variable / Placeholder** | A slot in a prompt template that gets filled at runtime with actual data (e.g., sensor reading, product name, customer query) |
| **Few-shot example** | One or more input→output examples embedded in a prompt to demonstrate the expected behavior to the model; part of the prompt asset |
| **Output schema** | A formal specification of what structure the AI must return (e.g., JSON schema with required fields and types); enforces predictable, machine-parseable outputs |
| **Guardrail** | An explicit prohibition or constraint in a prompt (e.g., "Do not provide financial advice"; "If data is insufficient, request more rather than guessing") |
| **Prompt registry** | A centralized, versioned repository where all managed prompt assets are stored, retrieved via API, and governed; analogous to an SOP document library |
| **Harness** | Every piece of code, configuration, and execution logic that wraps the model: system prompts, tool descriptions, orchestration logic, memory, sandboxes; makes a raw model into a working agent |
| **Dependency map** | Documentation of what a prompt/harness relies on: which data assets, glossary terms, tools, and eval datasets; enables impact analysis when dependencies change |
| **Prompt drift** | Silent degradation of prompt performance over time without any edit to the prompt, caused by model updates, input distribution shifts, or changes to dependent prompts |
| **Prompt regression** | Deliberate change to a prompt that causes worse performance; detected by running regression test suites before deploying a new version |
| **Prompt lifecycle** | The five-stage management process: Design → Test → Deploy (to registry) → Monitor → Maintain/Retire |

---

## 10. Easy-Understanding Summary for Writers

### The Core Insight (One Paragraph)

Organizations have decades of experience capturing human expertise as Standard Operating Procedures (SOPs): written, versioned, approved documents that encode how work should be done. AI doesn't change this need — it amplifies it. When an AI system is asked to make a verdict ("pass or fail?"), draft a document, or classify an event, it is executing a human judgment procedure. The prompt and harness that encode that procedure ARE a data asset — as important as the procedure itself. Without a prompt registry (governed SOP library), organizations discover their AI knowledge lives in personal accounts, hardcoded strings, and Slack threads. It disappears when employees leave. It varies by team. It can't be audited. Managing prompts and harnesses as organizational data assets is the infrastructure that turns individual AI experiments into institutional AI capability.

### Two Key Analogies

**Prompt registry ≈ SOP document library with version control**
- SOP has document number, version, author, approval, review date → prompt record has prompt ID, version hash, owner, approver, change history
- SOP library is searchable, governed, controlled → prompt registry is centralized, RBAC-protected, environment-promoted

**Harness ≈ Shift changeover SOP in manufacturing**
- When a shift changes, the outgoing operator documents: machine state, work done, anomalies, what needs doing next — so the incoming operator can start without memory loss
- Anthropic's harness for long-running agents does exactly this: `claude-progress.txt` captures work state, git history captures what was built, `init.sh` tells the next agent how to start — this is a harness encoding the shift-changeover procedure as AI-executable configuration

---

## Sources

| id | URL | Title |
|----|-----|-------|
| src-101 | https://www.langchain.com/blog/the-anatomy-of-an-agent-harness | The Anatomy of an Agent Harness (LangChain, Mar 2026) |
| src-102 | https://www.mongodb.com/company/blog/technical/agent-harness-why-llm-is-smallest-part-of-your-agent-system | The Agent Harness: Why the LLM Is the Smallest Part (MongoDB, Apr 2026) |
| src-103 | https://docs.langchain.com/langsmith/manage-prompts | Manage prompts — LangSmith Official Documentation |
| src-104 | https://www.kore.ai/blog/why-prompt-version-control-matters-in-agent-development | Prompt Version Control: The Missing Foundation (Kore.ai, Nov 2025 / Apr 2026) |
| src-105 | https://www.braintrust.dev/articles/best-prompt-versioning-tools-2025 | The 5 best prompt versioning tools in 2025 (Braintrust, Oct 2025) |
| src-106 | https://langfuse.com/docs/prompt-management/overview | Open Source Prompt Management — Langfuse Docs |
| src-107 | https://www.promptanthology.com/blog/enterprise-ai-prompt-governance | Enterprise AI Prompt Governance (PromptAnthology, Mar 2026) |
| src-108 | https://blog.neosage.io/p/the-prompt-lifecycle-every-ai-engineer | The Prompt Engineering Playbook / Prompt Lifecycle (NeoSage, Feb 2026) |
| src-109 | https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents | Effective harnesses for long-running agents (Anthropic, Nov 2025) |
| src-110 | https://agenta.ai/blog/prompt-drift | Prompt Drift: What It Is and How to Detect It (Agenta, Feb 2026) |
| src-111 | https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-management.html | Construct and store reusable prompts — Amazon Bedrock Docs (AWS) |
| src-112 | https://atlan.com/know/harness-engineering-vs-prompt-engineering/ | Prompt vs Context vs Harness Engineering: Key Differences (Atlan, Apr 2026) |
