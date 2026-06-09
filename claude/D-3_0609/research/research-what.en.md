# D-3 Prompt/Harness Asset Management — WHAT Cluster Research Notes

**Topic Code:** D-3  
**Topic Name:** Prompt/Harness Asset Management (Prompt/Harness 자산화)  
**Cluster:** WHAT (core — definition, structure, lifecycle, standards, glossary)  
**Author:** WHAT Research Agent  
**Date:** 2026-06-09  

---

## 1. Definition and Scope

### 1.1 What Prompt/Harness Asset Management IS

Prompt/Harness Asset Management is the systematic practice of treating AI instruction-and-judgment-logic artifacts — prompts and harnesses — as **managed organizational data assets** with versioning, metadata, registry, dependency mapping, and governance lifecycles.

**The core insight** (src-101, src-102, src-106): "Prompt management is to language models what Git is to software." Just as source code requires version control, code review, and deployment pipelines, prompts require the same engineering discipline because they are production dependencies whose behavioral changes have real downstream consequences.

**McKinsey 2024 data** (src-107): 78% of organizations now use AI in at least one business function — up from 55% the prior year. Yet most still treat prompts as disposable strings scattered across repositories, chat threads, and environment variables. The gap between AI experimentation and enterprise-scale deployment is precisely this lack of asset management infrastructure.

### 1.2 What It Is NOT

- **Not prompt engineering as a craft** — the goal here is not "how to write a cleverer prompt." It is how to capture, structure, version, register, and govern the prompt text artifact so it can be reliably reused.
- **Not agent architecture** — an AI agent is the *consumer* that executes the asset at runtime. The agent itself (its planning, reasoning, multi-step orchestration) is out of scope. This topic is about the asset the agent reads.
- **Not model fine-tuning** — fine-tuning changes how a model behaves; prompt asset management focuses on improving how teams prepare and maintain the instruction text delivered to that model.
- **Not tool/API management** — the Tool and API specifications the prompt may reference belong to D-2. Evaluation datasets referenced as dependencies belong to E-3.

### 1.3 The Central Problem Being Solved

Without prompt asset management, organizations face (src-106):

| Problem | Description |
|---|---|
| **Prompt Rot** | Behavior tightly coupled to a specific model version; silent regression when provider updates the model |
| **Versioning Black Hole** | Prompt versions scattered across repos, docs, chat threads; cannot determine which version is actually running |
| **Observability Black Box** | No telemetry; cannot track latency, token costs, or quality scores per prompt |
| **Economic Drain** | Bloated prompts not optimized; excessive token costs bleed budget |
| **Security Blindspots** | Unvalidated strings passed to LLM = prompt injection vulnerabilities |

---

## 2. Key Components and Structure

### 2.1 The Prompt Asset

A **prompt asset** is NOT just the instruction text. It is a composite, versioned artifact containing (src-100, src-101, src-105, src-106):

#### Canonical Prompt Asset Schema (synthesized from Langfuse, MLflow, Humanloop, NeoSage YAML)

```yaml
# Example: quality-inspection-judgment-prompt.v2.yaml
name: QualityInspectionJudgment            # Unique identifier
description: "Judges whether a surface defect requires rejection, rework, or pass based on SME criteria"
version: 2
commit_message: "Added crack size threshold from senior inspector interview 2026-05"
tags:
  - quality
  - inspection
  - casting-line-3
author: "kenji.tanaka@doosan.com"
task_domain: "quality-control"
status: "production"                        # draft | review | staging | production | deprecated

template_type: "chat"                       # text | chat

# The instruction/judgment-logic body
template:
  - role: "system"
    content: |
      You are an AI quality inspector assistant. Apply Doosan casting quality
      standards (rev. 2025-Q4) to evaluate surface defects.
      
      JUDGMENT CRITERIA (captured from senior inspector interview):
      - Cracks > 2mm depth: REJECT
      - Surface porosity > 3mm diameter: REWORK
      - Scratches < 0.5mm: PASS
      - Any defect near safety-critical zones (see zone_map): REJECT regardless of size
      
      FORBIDDEN ACTIONS: Do not pass components with any crack regardless of depth
      in zone A (structural load-bearing). Safety override always applies.
      
  - role: "user"
    content: |
      Defect report for component {{component_id}}:
      {{defect_description}}
      Zone: {{zone_code}}
      
      Provide judgment: PASS | REWORK | REJECT
      Reason: <one sentence>
      Confidence: <low|medium|high>

# Input variable specification
input_variables:
  - name: component_id
    type: string
    description: "Part serial number from MES"
  - name: defect_description
    type: string
    description: "Free text from optical inspection system or inspector notes"
  - name: zone_code
    type: string
    enum: ["A", "B", "C", "D"]
    description: "Zone code from engineering drawing"

# Output format specification
output_format:
  schema:
    judgment: {type: string, enum: [PASS, REWORK, REJECT]}
    reason: {type: string, max_length: 200}
    confidence: {type: string, enum: [low, medium, high]}

# Execution settings (versioned together with the prompt)
execution_settings:
  model: "claude-3-5-sonnet-20241022"
  temperature: 0.2          # Low temperature for deterministic quality judgments
  max_tokens: 150

# Dependency map
dependencies:
  data_sources:
    - "doosan-casting-quality-standards-2025-Q4"    # D-1 (data catalog reference)
    - "zone-map-engineering-drawings"
  evaluation_datasets:
    - "inspection-golden-dataset-v3"                # E-3 reference
  glossary_terms:
    - "surface-porosity"
    - "crack-depth"

# Governance
created_at: "2026-03-15"
updated_at: "2026-05-20"
reviewed_by: "quality-governance-council"
approved_for: ["casting-line-3", "casting-line-5"]
```

#### Minimum Required Fields for a Managed Prompt Asset
Based on convergence across Langfuse (src-100), MLflow (src-105), Humanloop (src-103), PromptLayer (src-104):

| Field | Description | Required? |
|---|---|---|
| `name` | Unique identifier (slug-style) | Yes |
| `version` | Immutable sequential integer | Yes |
| `template` | The instruction/judgment-logic body (text or chat array) | Yes |
| `input_variables` | Named placeholders with type and description | Yes |
| `output_format` | Expected response structure | Recommended |
| `execution_settings` | model, temperature, max_tokens | Recommended |
| `commit_message` | Description of what changed (like Git commit) | Recommended |
| `tags` | Key-value categorization for search and governance | Recommended |
| `status` / `labels` | Deployment stage pointer (production, staging, latest) | Yes |
| `dependencies` | Data sources, glossary terms, eval datasets referenced | Recommended |
| `author` | Who created/last modified this version | Yes |
| `description` | Human-readable purpose of the prompt | Yes |

### 2.2 The Harness Asset

A **harness** is a higher-order asset that packages multiple components into a single, repeatable work template. It bundles:

1. **One or more Prompt assets** (by name + version reference)
2. **Input-data specification** — what data needs to be fed in, from which system/schema
3. **Tool-call sequence** — ordered list of tools/APIs to invoke (references to D-2 tool specs)
4. **Output format specification** — the shape of the final output delivered to downstream systems
5. **Evaluation hook / acceptance criteria** — what constitutes a correct/acceptable execution (references E-3 eval datasets)

#### Harness Example: "Casting Defect Triage Harness"

```yaml
# casting-defect-triage-harness.v1.yaml
name: CastingDefectTriageHarness
description: "Complete work template for triaging surface defects on casting line 3. Bundles inspection judgment prompt + MES data retrieval + quality report generation."
version: 1

prompt_refs:
  - prompt: "QualityInspectionJudgment"
    version: 2
    role: "primary_judgment"
  - prompt: "QualityReportDrafting"
    version: 1
    role: "report_generation"

input_data_spec:
  source: "MES-casting-line-3"
  schema_ref: "defect-report-schema-v2"
  required_fields:
    - component_id
    - defect_photo_url
    - inspector_notes
    - zone_code

tool_call_sequence:
  - step: 1
    tool: "optical-inspection-api"
    purpose: "retrieve defect measurements from inspection camera"
  - step: 2
    tool: "zone-map-lookup"
    purpose: "identify zone from component_id"
  - step: 3
    prompt: "QualityInspectionJudgment@v2"
    purpose: "apply judgment criteria"
  - step: 4
    prompt: "QualityReportDrafting@v1"
    purpose: "generate standardized quality report"
  - step: 5
    tool: "MES-write-api"
    purpose: "write judgment result to MES"

output_format:
  to_system: "MES"
  schema_ref: "defect-decision-schema-v1"
  fields:
    - judgment: {type: string, enum: [PASS, REWORK, REJECT]}
    - reason: string
    - report_id: string

eval_hook:
  eval_dataset_ref: "inspection-golden-dataset-v3"
  acceptance_criteria:
    precision_on_REJECT: ">= 0.98"
    recall_on_REJECT: ">= 0.95"
```

**Analogy for manufacturing context:** A harness is like a digitized work instruction (SOP) for an AI — it specifies the exact sequence of steps, the data inputs required from each station, and the quality acceptance criteria, just as a paper work instruction specifies what a human operator must do at each step of the production line.

---

## 3. How It Works — The Prompt/Harness Asset Lifecycle

### 3.1 The Five-Stage Lifecycle (src-106, synthesized)

```
[1. CAPTURE] --> [2. STRUCTURE] --> [3. REGISTER] --> [4. GOVERN] --> [5. RETIRE/EVOLVE]
```

#### Stage 1: Capture — From Tacit Procedure to Structured Prompt

This is the most overlooked stage. Before a prompt can be managed, it must be **elicited** from a domain expert.

**The tacit knowledge problem** (src-108): In manufacturing, critical knowledge resides in experienced workers' heads — quality judgment criteria, maintenance heuristics, troubleshooting logic. This knowledge is:
- Not documented (or only partially in SOPs)
- Vulnerable to loss through retirement/turnover
- Difficult to articulate verbally

**AI-assisted capture method** (src-108): Conversational AI agents interview Subject Matter Experts (SMEs) and structure their responses into formal knowledge artifacts. Augmentir's "Augie" platform converts tacit manufacturing expertise into structured digital SOPs — the same pipeline applied to prompts.

**Capture checklist for a new prompt asset:**
- [ ] What task/judgment does this prompt perform?
- [ ] What are the explicit criteria/rules the SME applies? (judgment logic)
- [ ] What data does the SME look at to make this judgment? (input variables)
- [ ] What is the expected output format? (output schema)
- [ ] What are the hard constraints / forbidden actions? (guardrails)
- [ ] What are examples of correct and incorrect outputs? (golden cases for E-3)
- [ ] What data sources, terminology, or external standards does this depend on? (dependencies)

#### Stage 2: Structure — Decompose into Standardized Fields

Convert the captured knowledge into the canonical prompt schema (Section 2.1). Key structural decisions:
- **Text vs. Chat format** (src-100): Most complex judgment tasks benefit from chat format (system + user roles) because it separates the stable judgment criteria (system) from the variable inputs (user)
- **Variable decomposition**: Every dynamic data element becomes an explicit `input_variable` with a name, type, and description — not embedded as free text
- **Output schema definition**: Define the exact fields the AI should return, ideally with type constraints and enums

#### Stage 3: Register — Publish to Central Registry

Once structured, the prompt is registered in a **Prompt Registry** (analogous to a data catalog for prompts):

**Deployment workflow** (src-100, src-101, src-104):
1. Create new version (auto-gets `latest` label)
2. Test in playground / sandbox against representative inputs
3. Run against golden eval dataset (E-3 reference)
4. Promote: assign `staging` label --> validate --> assign `production` label
5. Application code fetches by label at runtime (decoupled from code deployment)

**Why decoupling matters** (src-101, src-109): When prompts are embedded in application code, a SME's wording change requires a full software deployment cycle (hours to days). With a registry, the SME (or prompt curator) can update the prompt independently, and the application picks it up on next fetch — without engineering involvement.

#### Stage 4: Govern — Version, Monitor, Dependency-Map

**Versioning principles** (src-100, src-105, src-101):
- Each change creates a new **immutable** version (never overwrite existing)
- Semantic versioning convention (src-107): MAJOR.MINOR.PATCH
  - MAJOR: Changes to output format or judgment logic that break downstream consumers
  - MINOR: New capabilities, improved criteria (backward compatible)
  - PATCH: Clarifications, typo fixes, minor wording
- All prior versions retained (rollback capability)
- Side-by-side diff for every version change

**Dependency mapping:**
- Each prompt records which data sources, glossary terms, and evaluation datasets it depends on
- When a dependency changes (e.g., quality standard is revised), dependency map surfaces which prompts need review

**Governance controls** (src-105, src-107):
- Role-based access: view / edit / promote / retire permissions
- Audit trail: every version change logged with author, timestamp, commit message
- Review workflow: changes above a certain severity require approval from domain owner
- Compliance: for regulated environments, link prompts to compliance requirements (ISO, QMS)

#### Stage 5: Monitor, Evolve, Retire

- Production execution traces are linked back to the exact prompt version that produced them (src-100, src-105)
- Quality metrics tracked per version: accuracy vs. golden cases, latency, token cost
- Low-scoring production cases fed back as new eval examples (feedback loop to E-3)
- Prompts deprecated when superseded or domain conditions change, but never deleted (regulatory audit requirement)

### 3.2 Packaging as a Harness

Once individual prompt assets are stable, they can be bundled into a **harness** — the packaged work template. This step corresponds to the transition from "I have a validated inspection judgment prompt" to "I have a complete, repeatable AI work procedure for the casting defect triage process."

Harness versioning follows the same principles as prompt versioning but at a coarser granularity — a MAJOR version bump when any constituent prompt changes its output schema.

---

## 4. Standards and Structures — PromptOps / Platform Asset Models

### 4.1 The PromptOps Concept

"PromptOps" (analogous to MLOps/DevOps) views prompts as operational assets that must be **governed, versioned, tested, and monitored** — not crafted as one-time artifacts. The Prompt Registry is the central infrastructure component, serving the same role as:
- A **version control system** (Git) for prompt text
- A **data catalog** for prompt metadata and discovery
- A **deployment manager** for environment promotion (dev --> staging --> production)

### 4.2 Platform Asset Models Comparison

**Note:** Humanloop shut down September 8, 2025 (src-103). The remaining platforms are all active.

| Platform | Vendor | Category | Key Asset-Management Features | Source |
|---|---|---|---|---|
| **Langfuse** | Langfuse GmbH (open source) | Open-source prompt registry + observability | Immutable versioned prompt objects; labels (production/staging/latest); chat + text types; variables + prompt composability; config versioned together with prompt; SDK caching; link traces to prompt versions | src-100 |
| **MLflow Prompt Registry** | Linux Foundation / Databricks | Open-source MLOps platform | Prompt object schema (name, template, version, commit_message, tags, alias, response_format, model_config); immutable versions; mutable aliases; Git-inspired diff; Unity Catalog governance; search by tags; lineage to experiments | src-105 |
| **PromptLayer** | PromptLayer Inc. | Hosted prompt CMS | Template registry with release labels (prod/staging); custom metadata per version; commit messages; threaded comments; A/B testing via Dynamic Release Labels; cost + latency analytics per version; lineage tracking | src-104 |
| **Arize AX / Phoenix** | Arize AI | Enterprise AI observability + prompt hub | Prompt Hub (versioned workspace); OpenInference schema (OTel-based); span replay; cross-version diff; prompt learning (automated scoring); Alyx AI-guided optimization; Apache Iceberg/Parquet export | src-102 |
| **Braintrust** | Braintrust Data Inc. | Evaluation-first prompt management | Versioned prompts as first-class objects; evaluation integrated into prompt lifecycle; CI/CD GitHub Action with quality gates; production monitoring dashboards; feedback loops (low-scoring traces --> new eval cases) | src-101 |
| **LangWatch** | LangWatch (Amsterdam) | Prompt registry + observability + evals | Immutable version IDs; CLI (`langwatch prompt sync`) for YAML-in-Git workflow; environment separation (dev/staging/prod); A/B + canary + feature flags; evaluation tied to prompt versions; ISO 27001 / SOC 2 | src-109 |
| **Amazon Bedrock Prompt Management** | AWS | Cloud-native managed service | Create/edit/version/share prompts; up to 3 parallel variations side-by-side; Prompt Optimization (auto-rewrite); metadata (author/department); integration with Bedrock Flows and Agents; IAM + CloudTrail governance; Generally Available Nov 2024 | src-110 |
| **BAML (BoundaryML)** | BoundaryML | Structured prompt DSL (file format) | `.baml` file = typed function (input params + output type + model config + prompt template); transpiles to Python/TS/Ruby/Java/C#/Rust/Go; LSP support; hot-reload; treats prompts as first-class code; versioned in Git | src-111 |
| **Humanloop** (shut down 2025) | Humanloop | Prompt management + evals | `.prompt` file format (YAML header + JSX body); version triggered by changes to template/model/params/tools; one Prompt File per task | src-103 |
| **DSPy** | Stanford NLP Group (open source) | Programmatic prompt optimization | Declarative modules with typed I/O signatures; compiler-based automatic prompt optimization; version-controlled logic; NOT a registry-based approach | src-102 |

### 4.3 Structured Prompt File Formats

Three file format conventions have emerged as practical standards for prompt assets stored in version control:

**1. YAML Frontmatter + Template Body (NeoSage / LangWatch style)** (src-106)
```yaml
name: SummarizeSupportTicket
version: 1
tags: ['support', 'summarization']
input_variables: [support_ticket_text, output_format]
execution_settings:
  model: "claude-3-opus-20240229"
  temperature: 0.5
  max_tokens: 512
template: |
  Your task is to extract information from the following support ticket.
  {{ support_ticket_text }}
```
Best for: Teams that want human-readable, Git-trackable prompt definitions.

**2. Humanloop `.prompt` Format (YAML header + JSX body)** (src-103)
```
---
model: gpt-4o
temperature: 1.0
max_tokens: -1
provider: openai
endpoint: chat
---
<system>
You are a {{persona}}. Answer questions in {{language}}.
</system>
```
Best for: Teams wanting separation of model config and prompt body.

**3. BAML Typed Function Format** (src-111)
```baml
function ExtractDefectReport(photo_url: string, inspector_notes: string) -> DefectReport {
  client Claude35Sonnet
  prompt #"
    Analyze this defect photo and inspector notes:
    {{ photo_url }}
    Notes: {{ inspector_notes }}
    Return structured data.
  "#
}
class DefectReport {
  defect_type string
  severity DefectSeverity
  recommended_action RecommendedAction
}
```
Best for: Developer-first teams wanting full type safety, multi-language code generation, and IDE tooling.

---

## 5. Dependency Map — What a Prompt/Harness Asset Depends On

A well-managed prompt asset maintains an explicit dependency map connecting it to:

| Dependency Type | Example | Managed Where |
|---|---|---|
| **Data sources** | MES defect table schema, engineering drawing zone map | D-1 (Data Catalog) |
| **Business glossary terms** | "surface porosity," "zone A" | D-x (Ontology/Glossary) |
| **Tool/API specs** | Optical inspection API response schema | D-2 |
| **Evaluation datasets** | Golden cases for inspection judgment | E-3 |
| **Model versions** | "claude-3-5-sonnet-20241022" | Model registry |
| **Other prompts** | Composite prompt references | Same registry |
| **Compliance standards** | ISO 9001 quality criteria | Governance metadata |

When any upstream dependency changes, the dependency map enables **impact analysis**: which prompts need review and re-evaluation?

---

## 6. Key Glossary (10 Terms)

### 6.1 Prompt Asset
The instruction-and-judgment-logic text artifact managed as a versioned, metadata-tagged organizational record. Distinct from a raw string: a prompt asset has a name, version, structured input variables, output schema, execution settings, dependency map, and governance status. **Manufacturing analogy:** A digitized SOP or work instruction that specifies how a task must be performed, with the revision history and approvals recorded.

### 6.2 Harness
A packaged, reusable work template that bundles a prompt + input-data specification + tool-call sequence + output format + evaluation criteria into one repeatable "work procedure" asset. **Manufacturing analogy:** A complete job package (work instruction + BOM + quality acceptance criteria + equipment setup sheet) for a specific production task — everything an operator needs to execute the job.

### 6.3 Prompt Registry
A centralized, searchable repository where prompt assets are stored, versioned, discovered, and fetched at runtime. Functions as both a content management system and a deployment manager (environment promotion). **Manufacturing analogy:** A controlled document repository (DMS) for all active work instructions, with version control and access management.

### 6.4 Prompt Versioning
The practice of creating a new immutable record every time a prompt is changed, preserving all prior versions with full audit trail (who changed what, when, why). Every production execution can be traced to the exact prompt version that produced it. (src-100, src-105) **Manufacturing analogy:** Engineering change order (ECO) management, where each change to a drawing creates a new revision while the old revision is archived.

### 6.5 Prompt Template / Schema
The standardized structural definition of a prompt asset — the required fields (name, version, template body, input variables, output format, execution settings, metadata) that every managed prompt must have. Ensures prompts are self-describing, machine-readable, and consistently formatted. **Manufacturing analogy:** A standard form template for work instructions — every WI must have the same sections (purpose, scope, tools required, step sequence, acceptance criteria).

### 6.6 System Prompt (Role Instruction)
In a chat-format prompt asset, the `system` role message that defines the AI's persona, task scope, judgment criteria, and forbidden actions. This is where the captured SME judgment logic lives — the stable, organization-approved part of the instruction that changes less frequently than user-turn input variables.

### 6.7 Dependency Map
An explicit record of every external asset a prompt depends on: data sources, glossary terms, tool/API specs, evaluation datasets, other prompts, and compliance standards. Enables impact analysis when upstream assets change. **Manufacturing analogy:** Bill of materials (BOM) reference on a work instruction — records exactly what materials, tools, and supporting documents the procedure requires.

### 6.8 Golden Prompt / Reference Prompt
A prompt version that has been formally validated against a curated evaluation dataset and approved as the baseline for comparison. When a new version is developed, its performance is measured against the golden prompt's scores. (src-106) **Manufacturing analogy:** A "golden sample" or limit sample in manufacturing quality control — the approved reference piece against which new production is compared.

### 6.9 Label / Alias (Deployment Tag)
A mutable pointer attached to a specific prompt version indicating its deployment environment (production, staging, latest). The application code references the label, not the version number — so promoting a new version to production requires only a label reassignment, not a code change. (src-100, src-104) **Manufacturing analogy:** A "current revision" indicator on a document — the document number stays the same; only the revision letter/number and the "current" flag change.

### 6.10 Prompt Drift
The phenomenon where a prompt's actual behavior changes over time without any deliberate change to the prompt text — caused by model updates, changes in upstream data distributions, or shifts in the domain context. Managed through production monitoring and regression testing against golden cases. **Manufacturing analogy:** Sensor calibration drift in process instrumentation — the instrument physically did not change, but its readings gradually diverge from truth.

---

## 7. Easy-Understanding Material: Analogies and Examples

### 7.1 The SOP Library Analogy
A prompt registry is like a **version-controlled SOP library** in manufacturing:
- SOPs have document numbers (= prompt names)
- Each revision has a version number and a change log (= commit message)
- Only the approved current revision is used on the line (= "production" label)
- All prior revisions are archived, not deleted (= immutable version history)
- Each SOP references the materials and equipment it uses (= dependency map)
- New SOPs go through review and approval before use (= governance workflow)

The key difference: prompt assets carry the exact model configuration (temperature, max tokens) and machine-readable input/output schemas — making them directly executable, not just readable.

### 7.2 Manufacturing Use Case Examples

**Example 1: Quality Defect Cause Analysis Prompt**
- **SME knowledge captured:** Senior quality engineer's classification logic for casting defect root causes (tooling wear, material spec deviation, operator error)
- **Prompt asset structure:** System message encodes the classification criteria and decision tree; user message template injects the defect measurement data and shift log
- **Dependency map:** References casting quality standards document + defect classification taxonomy (glossary)
- **Harness:** Bundles the classification prompt + MES data query tool + defect report drafting prompt + QMS write-back tool

**Example 2: Inspection Judgment Harness**
- **SME knowledge captured:** Visual inspection gate decision criteria (PASS/REWORK/REJECT) for hydraulic component surfaces
- **Prompt asset structure:** System message encodes dimensional tolerances and visual criteria; input variables: component_id, defect_description, zone_code; output schema: {judgment, reason, confidence}
- **Dependency map:** References engineering zone map + quality standard rev 2025-Q4
- **Harness:** Packages judgment prompt + optical measurement API call + REJECT escalation notification prompt + MES write-back

**Example 3: C/S Report Drafting Harness**
- **SME knowledge captured:** Customer service agent's procedure for drafting warranty claim responses (policy, tone, escalation criteria)
- **Prompt asset structure:** System message encodes response policy and forbidden commitments; input variables: claim_type, product_model, failure_description; output schema: {response_draft, escalation_flag, escalation_reason}
- **Harness:** Bundles claim classification prompt + product history CRM lookup + response drafting prompt + supervisor routing logic

### 7.3 Cause-Effect Chain for "Why Asset Management"

**Scenario A — Without prompt asset management:**
1. Senior quality engineer teaches the AI team how to classify defects (one-time verbal session)
2. Team writes a prompt string hard-coded in the inspection application
3. 6 months later: engineer retires; nobody can explain the classification logic
4. Model provider updates the model; classification rates shift silently
5. Defect escapes noticed 3 weeks later in field returns
6. Team cannot reproduce the original behavior (no version history)

**Scenario B — With prompt asset management:**
1. Engineer's judgment criteria are elicited and structured into a versioned prompt asset with dependency map
2. Prompt is registered with golden eval dataset; deployed to production via label
3. Engineer retires; all knowledge is in the registry, traceable to the interview date
4. Model update detected: automated regression test against golden cases shows accuracy drop
5. Previous version reinstated instantly via label reassignment (no code change, 2 minutes)
6. New version developed with updated criteria, reviewed by incoming engineer, promoted

---

## 8. Detailed Reference Material (Backup)

### 8.1 Full Field Schema Reference — MLflow Prompt Object

From src-105 (MLflow Prompt Registry official documentation):

```python
# Complete MLflow Prompt Object fields:
mlflow.genai.register_prompt(
    name="qa-prompt",                          # Unique identifier
    template="Answer: {{question}}",           # Template (text or chat list)
    commit_message="Initial version",          # Git-style commit description
    tags={                                     # Key-value categorization
        "author": "engineer@company.com",
        "task": "question-answering",
        "language": "en",
        "domain": "quality-control",
    },
    model_config={                             # Model settings (versioned with prompt)
        "model_name": "gpt-4",
        "temperature": 0.7,
        "max_tokens": 1000,
        "top_p": 0.9,
        "frequency_penalty": 0.0,
        "presence_penalty": 0.0,
        "stop_sequences": ["END"],
    },
    # response_format: Pydantic model or JSON schema dict for output validation
)
```

**Alias management for deployment:**
```python
# Set production alias
mlflow.set_prompt_alias("qa-prompt", "production", version=5)
# Application loads by alias (not version number) -- deployment decoupled from code
prompt = mlflow.genai.load_prompt("prompts:/qa-prompt@production")
```

**Search capability:**
```python
prompts = mlflow.genai.search_prompts(filter_string="task='quality-control'")
```

### 8.2 Langfuse Versioning Model Details

From src-100 (Langfuse official docs):

- **Version:** Immutable integer. Every save = new version (1, 2, 3...).
- **Labels:** Mutable pointers. Application code references labels, not version numbers.
  - `production` — default deployment label
  - `latest` — always points to newest version
  - Custom labels for staging, testing, A/B tests, tenant-specific versions
- **Caching:** Prompts cached client-side by SDK. Production fetch = memory read speed (zero added latency). TTL configurable per label type.
- **Composability** (unique Langfuse feature): Prompts can reference other prompts via Prompt References, enabling component reuse. Example: a "tone-guidelines" prompt referenced inside multiple customer-facing prompts — change tone policy in one place, all consumers pick it up.

### 8.3 PromptLayer Registry Feature Details

From src-104 (PromptLayer official registry docs):

- **Template formats:** f-string `{variable}` OR Jinja2 `{{variable}}` (choice per template)
- **Release labels:** Must be unique across all versions (enforced constraint prevents ambiguity)
- **Commit messages:** Max 72 characters (Git convention)
- **Custom metadata:** Any key-value pair per version: provider, model, temperature, category, business_unit, approved_by, etc.
- **Threaded comments:** Collaborative discussion threads on prompt versions (knowledge preservation)
- **A/B testing:** Dynamic Release Labels split traffic by percentage or user segment
- **Analytics per version:** average quality score, latency distribution, cost tracked automatically

### 8.4 Humanloop .prompt File Format (Historical Reference)

From src-103 (Humanloop official docs — company shut down Sep 2025):

The `.prompt` file format defined a serialization standard:
```
---
model: gpt-4o
temperature: 1.0
max_tokens: -1
provider: openai
endpoint: chat
---
<system>
You are a {{persona}}. Answer questions in {{language}}.
</system>
```

**What triggers a new version (Humanloop's exact rule):** Any change to template, model, parameters (temperature, max_tokens, top_p), or available tools.

**Design principle stated:** "Separation of concerns — keeping configuration separate from the query time data is crucial for enabling experimentation."

**One Prompt File = One Task:** Each distinct task (Writing Copilot, Personal Assistant, Summarizer) gets its own Prompt File with its own version history. All versions of a task should be interchangeable (same I/O contract).

### 8.5 Enterprise Portfolio Framework

From src-107 (Segev Shmueli / Medium, July 2025):

**Industry benchmark KPIs:**
- Prompt reuse rate across departments: 40-60% (industry benchmark)
- Time-to-production for new AI capabilities: Target weeks --> days
- Average prompt engineering session duration: 43.3 minutes
- Time between prompt versions: ~50 seconds (highly iterative)
- 93% of sessions involve parameter or model tweaks beyond just text changes

**Semantic versioning for prompts (X.Y.Z):**
- X (MAJOR): Breaks downstream dependencies (output format change, fundamental logic change)
- Y (MINOR): New capabilities, backward compatible
- Z (PATCH): Bug fixes, clarity improvements

**Governance roles:**
- Business Process Owners --> define requirements and success criteria
- Prompt Engineers --> technical implementation and optimization
- Portfolio Governance Council --> cross-functional oversight (architecture, security, strategy)
- Quality Assurance Teams --> evaluation (human + automated)

**Deployment strategies:**
- Canary Releases: Gradual rollout to subset of users/transactions
- Blue-Green Deployment: Parallel environments for instant rollback
- Feature Flags: Runtime switching between prompt versions based on conditions

### 8.6 AWS Well-Architected Framework — Prompt Template Management as Standard Practice

From src-110 (Amazon Bedrock Prompt Management):

Amazon Bedrock Prompt Management became Generally Available in November 2024. Its inclusion in the AWS **Well-Architected Generative AI Lens** as "GENOPS03-BP01 Implement prompt template management" signals that prompt asset management is now considered a **standard operational best practice** for production AI systems — not an optional improvement.

AWS features:
- Create/edit/version/share prompts across teams
- Up to 3 parallel prompt variations for A/B comparison
- Automatic prompt optimization (rewrite for better accuracy)
- Enterprise metadata tracking (author, department)
- Integration with Bedrock Flows and Bedrock Agents
- Governance via AWS IAM (access control) + CloudTrail (audit trail)

---

## Sources

| ID | URL | Title | Used In |
|---|---|---|---|
| src-100 | https://langfuse.com/docs/prompt-management/data-model | Concepts – Langfuse | Sections 2.1, 3.1, 6.4, 8.2 |
| src-101 | https://www.braintrust.dev/articles/what-is-prompt-management | What is prompt management? – Braintrust | Sections 1.1, 1.3, 2.1, 3.1, 4.2 |
| src-102 | https://arize.com/blog/top-5-ai-prompt-management-tools-of-2025/ | Top 5 AI Prompt Management Tools of 2025 – Arize AI | Sections 1.1, 2.1, 4.2, 6.10 |
| src-103 | https://humanloop.com/docs/explanation/prompts | Prompts – Humanloop Docs | Sections 2.1, 4.2, 4.3, 8.4 |
| src-104 | https://docs.promptlayer.com/features/prompt-registry/overview | Prompt Registry Overview – PromptLayer | Sections 2.1, 3.1, 4.2, 6.3, 8.3 |
| src-105 | https://mlflow.org/docs/latest/genai/prompt-registry/ | Prompt Registry – MLflow AI Platform | Sections 2.1, 3.1, 4.2, 6.1, 6.9, 8.1 |
| src-106 | https://blog.neosage.io/p/the-prompt-lifecycle-every-ai-engineer | The Prompt Lifecycle – NeoSage / Dimple Sharma | Sections 1.3, 2.1, 3.1, 4.3, 6.10 |
| src-107 | https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327 | Managing Prompts as Enterprise Assets – Segev Shmueli | Sections 1.1, 4.2, 8.5 |
| src-108 | https://www.augmentir.com/glossary/tacit-knowledge | Tacit Knowledge in Manufacturing – Augmentir | Sections 3.1, 7.2 |
| src-109 | https://langwatch.ai/blog/what-is-prompt-management-and-how-to-version-control-deploy-prompts-in-productions | What is Prompt Management? – LangWatch | Sections 1.1, 2.1, 3.1, 4.2 |
| src-110 | https://aws.amazon.com/bedrock/prompt-management/ | Amazon Bedrock Prompt Management | Sections 4.2, 8.6 |
| src-111 | https://github.com/BoundaryML/baml | BAML – BoundaryML | Sections 2.2, 4.2, 4.3, 6.2 |
