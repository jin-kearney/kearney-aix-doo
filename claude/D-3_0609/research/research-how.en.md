# D-3 Prompt/Harness Asset Management — Research Notes: HOW (Data-Prep Methodology)

**Topic code**: D-3  
**Topic name**: Prompt/Harness Asset Management  
**Researcher**: AI Research Agent  
**Date**: 2026-06-09  
**Scope**: HOW cluster — how to prepare, curate, structure, and govern the prompt/harness as a managed data asset

---

## 1. Framing: The Core Problem This Topic Solves

Most organizations today treat prompts as disposable text snippets—scattered across Slack messages, Jupyter notebooks, shared drives, and individual laptops. When AI initiatives scale beyond a single developer, this ad-hoc approach breaks down catastrophically:

- A 2024 analysis of AI pilot programs found that **95% fail to deliver measurable impact**, with unmanaged prompt changes contributing significantly to this failure rate. ([src-164])
- Research on enterprise prompt editing sessions found the average session lasts **43.3 minutes** with approximately **50 seconds between versions**; and **93% of sessions** involve parameter or model tweaks beyond just text changes, indicating the complexity of prompt assets that teams fail to capture systematically. ([src-154])
- McKinsey's 2025 State of AI survey: **71% of organizations** use generative AI in at least one business function, yet most treat prompts as disposable scripts rather than strategic assets. ([src-154])

The DATA-layer solution: treat the prompt (and the harness that packages it with its inputs, tools, and eval hooks) as a **managed organizational asset**—captured, structured, versioned, governed, and discoverable—just as source code or data schemas are managed.

---

## 2. The Asset-ification Pipeline: 7 Ordered Stages

The following is an ordered pipeline for converting tacit work procedures into managed prompt/harness assets. Each stage corresponds to a data-preparation workstream.

### Stage 1: ELICIT / CAPTURE — Converting Tacit SME Procedures into Raw Material

**The problem**: Expert judgment and work procedures exist in SMEs' heads, not in structured form. Before a prompt can be written, the underlying logic must be extracted.

**Methods**:
- **Structured interviews**: Ask the SME to walk through a decision scenario step by step. Use the "Think Aloud" technique (ask the SME to narrate their reasoning while performing the task). ([src-166])
- **Conversational AI-assisted extraction**: LLM-driven conversational assistants can capture tacit knowledge on the shop floor and convert it incrementally and interactively into structured process documents. ([src-166])
- **Recording + AI transcription**: Record verbal walkthrough, then use AI to convert the recording into a structured first draft in under 1 minute. ([src-166])
- **Observation + process archaeology**: Watch the expert perform the task; extract patterns, exceptions, reference criteria, and boundary conditions.

**Output of Stage 1**: A raw, unstructured description of the work procedure—what inputs arrive, what judgment steps are performed, what reference criteria are applied, what output is expected, and what forbidden actions exist.

**Manufacturing example**: A quality inspector's defect judgment: "When I see surface marks, I first check if they're within the 0.3mm tolerance spec in document QC-12. If borderline, I check for location—edge defects get a pass, center defects do not. I also check if the part is for a safety-critical assembly…"

### Stage 2: DECOMPOSE / STRUCTURE — Breaking Raw Procedure into Standard Fields

**The problem**: Raw verbal descriptions are unstructured and cannot be consistently executed or validated by an AI system. They must be decomposed into machine-readable standard fields.

**Standard Prompt Asset Fields** (synthesized from [src-155], [src-156], [src-157], [src-152]):

| Field | Description | Example |
|-------|-------------|---------|
| `purpose` | The task outcome the prompt is meant to achieve | "Classify incoming material defect reports as accept/hold/reject using QC standard doc QC-12" |
| `inputs` | What structured data, documents, or context the prompt requires | Defect photo + measurement report + part number + assembly destination |
| `judgment_steps` | The ordered decision logic (what the SME actually evaluates) | Step 1: Check if measurement is within tolerance. Step 2: Check defect location. Step 3: Check safety-critical flag |
| `reference_criteria` | Thresholds, standards, lookup tables, or glossary terms applied | QC-12 tolerance table v3.2; Safety parts registry |
| `output_format` | What the response must look like (structure, schema, fields) | JSON: {decision: "accept|hold|reject", reason: string, confidence: 0–1} |
| `forbidden_actions` | What the prompt must never do | Must not accept if safety-critical flag = true, regardless of measurement |
| `examples` | Few-shot or golden examples (input–output pairs) | 3 confirmed accept, 2 confirmed hold, 2 confirmed reject |
| `model_constraints` | Which model and parameter settings this prompt is designed for | gpt-4o, temperature=0.2, max_tokens=500 |

**SPDD REASONS Canvas** (Thoughtworks Structured Prompt-Driven Development, [src-152]):  
A more complete structured decomposition framework from Thoughtworks' internal engineering practice:
- **R** — Requirements: What problem are we solving? What is the Definition of Done?
- **E** — Entities: Domain entities and relationships.
- **A** — Approach: The strategy for meeting the requirements.
- **S** — Structure: Where the change fits in the system; components and dependencies.
- **O** — Operations: Break abstract strategy into concrete, testable implementation steps.
- **N** — Norms: Cross-cutting engineering norms (naming, observability, defensive coding).
- **S** — Safeguards: Non-negotiable boundaries (invariants, performance limits, security rules).

Key SPDD principle: "Because the structured prompt captures the full specification, reviewers reason about a single artifact instead of scattered chat logs and partial diffs." ([src-152])

### Stage 3: STANDARDIZE — Prompt Templates and Schemas

**The problem**: Each team writes prompts in different styles, with different variable placeholders, different section ordering, and no enforcement. This creates a non-interoperable collection that cannot be governed uniformly.

**Standardization approaches**:

**Template-based standardization**: Define a canonical prompt template with typed variable slots. The template enforces what sections must be present and in what order. ([src-153])

**BAML (Basically a Made-up Language)** ([src-158]): A domain-specific prompt file format where the schema and the prompt live in the same file, preventing drift between specification and implementation. Key benefits:
- Co-located, co-versioned schema and prompt (cannot drift apart)
- Provides autocomplete and static analysis
- Parser generates an error if LLM output cannot be parsed against the schema
- 60% fewer tokens than equivalent JSON schemas
- Transpiles to Python, TypeScript, Ruby, Go, and other languages
- Files can be checked into GitHub for easy diffs

**Other prompt file formats**: `.prompt` files, YAML front matter with prompt body, JSON schemas for output validation. The key principle is that the schema is machine-readable and attached to the prompt, not in a separate document. ([src-157])

**Standardization checklist** (before a prompt can enter the registry):
- [ ] All required metadata fields present (see Stage 4)
- [ ] Variable placeholders use consistent syntax (e.g., `{{variable_name}}`)
- [ ] Output format is a defined schema (not free text)
- [ ] At least 3 golden examples attached
- [ ] Glossary terms in the prompt match the organizational glossary (D-2/A-3)
- [ ] Forbidden actions explicitly stated
- [ ] No hardcoded secrets, API keys, or PII embedded in template ([src-165])

### Stage 4: ATTACH METADATA AND REGISTER — The Prompt Registry

**The problem**: Even structured prompts stored in shared folders have no version history, no owner, no change reason, and no way to track what task they're applied to or who approved them.

**Core Metadata Schema** (synthesized from [src-150], [src-156], [src-157]):

| Metadata Field | Type | Description |
|----------------|------|-------------|
| `prompt_id` | string | Unique identifier (e.g., "qc-defect-classify-v3") |
| `name` | string | Human-readable name |
| `version` | semver | e.g., v2.1.0 (major.minor.patch) |
| `commit_message` | string | Why this version was created |
| `author` | string | Who created this version |
| `owner` | string | Team/email accountable for maintenance and escalations |
| `approver` | string | Who approved this version for production |
| `status` | enum | draft / review / staging / production / deprecated |
| `applied_task` | string | What real-world task this prompt executes |
| `domain` | string | Business domain (e.g., quality-control, finance, customer-service) |
| `intent` | string | What the prompt achieves (classify, summarize, extract, generate, plan) |
| `model_config` | dict | Target model, temperature, max_tokens, stop_sequences, etc. |
| `created_at` | timestamp | When first registered |
| `updated_at` | timestamp | When last modified |
| `tags` | list | Freeform tags for discovery |
| `change_reason` | string | Business or quality reason for this version vs. previous |
| `deprecation_date` | date | When this prompt will be retired (if applicable) |

**MLflow Prompt Registry** ([src-150]) provides these fields natively: name, template, version (sequential), commit_message, tags (key-value), alias (mutable named reference), response_format, model_config (model_name, temperature, max_tokens, top_p, top_k, frequency_penalty, presence_penalty, stop_sequences, extra_params).

**Langfuse version/label model** ([src-151]): versions are immutable (1, 2, 3…); labels are mutable pointers to versions (e.g., "production", "staging"). Protected labels can be locked by admins.

**LangSmith Prompt Hub** ([src-160]): commit hash per version, commit tags as named aliases, environment management (staging/production).

**Semantic versioning convention** ([src-154], [src-156]):
- **Major** (X.0.0): Breaking changes — fundamental restructuring, output format changes that break downstream consumers.
- **Minor** (x.Y.0): Backward-compatible improvements — additional instructions, refined wording, new examples.
- **Patch** (x.y.Z): Bug fixes — typo corrections, formatting adjustments, minor clarifications.

### Stage 5: MAP DEPENDENCIES — What the Prompt Needs to Run

**The problem**: A prompt silently depends on specific data assets, glossary terms, tool specs, and eval sets. When any dependency changes, the prompt may break or produce incorrect results—with no warning.

**Why dependency mapping is critical**:
"A hidden web of dependencies exists where one changed upstream artifact can compromise downstream applications. Mismanagement of dependencies causes disruptions when a dependency changes but not all dependents are known." ([src-165], adapted)

**Dependency types for a prompt asset**:

| Dependency type | Examples | What to track |
|-----------------|----------|---------------|
| **Data assets** | Lookup tables, reference documents, master data | Asset ID, version, location, refresh frequency |
| **Glossary terms** | Domain-specific terminology used in the prompt | Term, definition source (D-2/A-3 glossary) |
| **Tool/API specs** | Functions the prompt instructs the agent to call | Tool name, API version, endpoint (reference D-2) |
| **Eval sets** | Golden examples and test cases | Eval dataset ID, version, coverage (reference E-3) |
| **Model** | Which LLM and version this prompt was validated against | Model name, version, provider |
| **Downstream prompts** | Other prompts that consume this prompt's output | Prompt ID, which field is consumed |

**Dependency map format** (YAML front matter example):
```yaml
---
prompt_id: qc-defect-classify-v3
version: 2.1.0
dependencies:
  data:
    - id: qc-standard-doc
      version: QC-12 v3.2
      location: data-catalog://qc/QC-12
  glossary:
    - term: "safety-critical assembly"
      source: glossary://manufacturing/safety-terms
  tools:
    - id: measurement-api
      version: v2.3
      spec_ref: D-2://measurement-api
  eval_sets:
    - id: defect-classification-golden
      version: v1.4
      ref: E-3://qc-defect-golden
  model:
    name: gpt-4o
    version: 2025-11-01
---
```

AWS Prescriptive Guidance mandates: "Log all prompts, parameters, and model responses" and "Track model versions and provider updates" because "models are updated frequently. Knowing the version you're using is essential for reproducibility, evaluation, and cost impact analysis." ([src-153])

### Stage 6: PACKAGE AS A HARNESS — Bundling into a Reusable Unit

**The problem**: A prompt template alone is not self-contained. To reliably re-execute a task, you need the prompt + input schema + tool/function sequence + output schema + eval hooks bundled together as a single, reusable artifact.

**Harness definition** (synthesized from [src-163], [src-153]):
A harness is the execution environment *around* the LLM that packages:

| Harness Layer | Contents | Asset management focus |
|---------------|----------|------------------------|
| **Model Interface** | Prompt template, parameter configuration, response parsing rules, error handling | The prompt asset itself (Stages 1–5) |
| **Input Schema** | What data inputs are required, their types and validation rules | Input contract: what must be present before the harness can run |
| **Tool Sequence** | Which tools/APIs are called, in what order, with what parameters | Tool dependency map (reference D-2 for specs) |
| **Output Schema** | Expected output structure, validation rules | Defines what downstream consumers receive |
| **Eval Hooks** | Pre-defined test suite, golden cases, regression triggers | Connects to E-3 eval dataset; runs after execution |
| **Fallback Logic** | What to do if model confidence is low or output fails validation | Defined in harness, not left to runtime improvisation |
| **Human Approval Gates** | When to escalate to human review instead of proceeding | Defined governance boundaries |

**Harness Asset Metadata** (beyond prompt metadata):
- `harness_id` / `name`
- `prompt_ref`: pointer to the prompt asset (prompt_id + version)
- `input_schema_version`
- `output_schema_version`
- `tool_refs`: list of tool IDs and versions
- `eval_set_ref`: eval dataset ID and version
- `fallback_behavior`: documented
- `approval_required`: boolean + conditions

**Manufacturing example — "Incoming Inspection Judgment Harness"**:
```
harness_id: incoming-inspection-v2
prompt_ref: incoming-inspection-prompt@v2.1.0
inputs:
  - material_id: string
  - inspection_report: PDF path
  - part_number: string
  - safety_critical: boolean
tools:
  - measurement_db_query (get tolerance spec by part number)
  - material_master_lookup (get part classification)
output_schema:
  decision: "accept|hold|reject"
  reason: string
  confidence: float
  reviewer_required: boolean
eval_hooks:
  - golden_cases_v1.4 (run regression after each prompt version change)
fallback:
  if confidence < 0.7: escalate_to_human_reviewer
  if output_parse_error: log_and_alert, return "hold"
```

Harness Engineering principle: "It treats the model as a component — powerful but incomplete — and focuses on everything else." ([src-163])

AWS Prescriptive Guidance mandates: "Establish confidence thresholds and fallback behavior. If a model's confidence is low or the output is ungrounded, route to a human, a static rule, or a simpler workflow." ([src-153])

### Stage 7: VERSION AND GOVERN CHANGES — Change Lifecycle Management

**The problem**: Even well-structured prompts become unmanaged over time without a formal change governance workflow—no change reason recorded, no approver on record, no rollback path, no way to audit who changed what and why.

**Core governance mechanisms**:

**1. Change control process** ([src-153], [src-154]):
- All changes to prompt assets go through: Create branch → Modify prompt → Add change reason → Run tests → Open PR → Domain owner review → Automated evaluation checks → Merge to staging → Shadow mode validation → Promote to production
- Prompt changes must pass automated evaluation before deployment (regression testing against golden eval set)
- Pull-request style reviews: reviewers check naming, taxonomy fit, policy coverage, and evaluation results ([src-157])

**2. Environment promotion model** ([src-150], [src-151], [src-153]):
- **draft**: Being written; not ready for review
- **staging**: Validated in staging environment; being tested against production traffic shadow
- **production**: Active alias pointing to the approved version; protected label
- **deprecated**: No longer recommended; still accessible for a defined window

**3. Rollback procedures** ([src-155], [src-156]):
- One-click rollback: Restore previous production alias/label to point to previous version
- Automatic rollback triggers: Define quality thresholds that auto-revert when metrics degrade
- Version pinning: Explicit specification of which prompt versions serve which applications or features

**4. Approval workflow roles** ([src-154], [src-157]):
- **SME (Subject Matter Expert)**: Validates that the prompt logic correctly captures the domain procedure and judgment criteria
- **Data Steward / Prompt Engineer**: Ensures structural correctness—standardized fields, metadata completeness, dependency map accuracy
- **Approver / Domain Owner**: Signs off that the prompt is fit for production use
- **Portfolio Governance Council** (enterprise scale): Cross-functional oversight ensuring alignment with enterprise architecture, security standards, and strategic objectives

**5. Audit trail requirements** ([src-159], [src-153]):
"Organizations that cannot demonstrate how their AI systems were instructed, by whom, under what approval process, and with what monitoring will find themselves exposed under multiple overlapping regulatory regimes." ([src-159])
- Immutable version records: Once created, a version cannot be altered (MLflow, Langfuse both enforce this) ([src-150], [src-151])
- Main EU AI Act compliance date for high-risk systems: August 2, 2026 — making prompt audit trails urgently needed ([src-159])

---

## 3. Data Workstreams: What to Actually DO to the Asset

### Converting a Verbal/Handwritten SOP into Structured Prompt Fields

Step-by-step process ([src-166], [src-152]):
1. Record SME verbal walkthrough or digitize existing paper SOP
2. Use AI to transcribe and produce a first-draft structure (< 1 minute)
3. Human review: identify judgment steps, reference criteria, edge cases
4. Map to standard prompt field schema (Stage 2 above)
5. Write 5–7 golden examples (confirmed input→output pairs from historical cases)
6. Validate with SME: "Does this correctly capture how you would evaluate this scenario?"
7. Attach metadata, register in prompt registry

### De-duplicating Near-Identical Prompts

"Prompt sprawl" is the enterprise norm before governance: multiple versions of the same prompt scattered across teams, each with slight variations. De-duplication process:
1. **Discovery scan**: Pull all prompt files from repos, notebooks, shared drives
2. **Semantic clustering**: Group by intent + domain + task; identify near-duplicates
3. **Diff and reconcile**: Use diff views (available in MLflow, Langfuse, Agenta) to compare; identify which version performs best
4. **Canonical version selection**: Pick the best-performing version as the master
5. **Migration**: Update all consumers to point to the canonical registry version
6. **Deprecate the others** with migration notices

### Standardizing Terminology in Prompts to Match the Glossary

Cross-reference all domain terms used in prompt templates against the organizational glossary (A-3). Common issues:
- Using "defect" when the glossary says "nonconformance"
- Using model-provider-specific jargon that doesn't match internal terminology
- Using ambiguous terms without definition that different SMEs interpret differently

Fix: Run a terminology audit pass before registering. Add glossary term IDs to the dependency map.

### Tagging Prompts by Task/Domain

Use a three-facet taxonomy ([src-157], [src-167]):
- **Intent**: classify / summarize / extract / generate / plan / judge / draft / validate
- **Domain**: quality-control / finance / customer-service / logistics / hr / legal / marketing
- **Modality**: text-to-text / text-to-json / document-to-text / multi-turn

Naming convention: `intent.domain.modality.task-name.vX` — Example: `classify.quality-control.text-to-json.defect-severity.v2`

### Writing a Dependency Map

See Stage 5. Key discipline: the dependency map must be updated BEFORE a prompt version is promoted to production. If a data asset version changes (e.g., QC-12 updated to v3.3), the dependency map must be reviewed and the prompt re-validated.

### Building a Reference/Golden Prompt Set

A "golden prompt set" is a curated collection of known-good prompts that:
- Have been validated by SMEs and domain owners
- Have established performance baselines (accuracy, latency, cost)
- Serve as references for new prompt development
- Form the regression test baseline (run against these when any change is made)

---

## 4. Manual Pain Points and How Automation Helps

### Pain Points (Without a Prompt Registry)

| Pain point | Description |
|------------|-------------|
| **No version history** | "It worked yesterday!" — cannot identify what changed ([src-155]) |
| **No owner** | When a prompt breaks, no one is accountable ([src-157]) |
| **Copy-paste drift** | Same prompt copied into 12 different scripts, each modified slightly; 12 incompatible versions exist ([src-162]) |
| **Hidden dependencies** | When the underlying data spec changes, the prompt silently produces wrong results ([src-156]) |
| **Secrets in prompts** | API keys, PII, or internal system names embedded in prompt templates; replication across repos and logs ([src-165]) |
| **No single source of truth** | Prompts in Slack, notebooks, shared folders, individual email — no discovery ([src-154]) |
| **Terminology inconsistency** | The prompt says "defect" but the model says "nonconformance" — confusion for downstream consumers |
| **Untracked changes** | Someone edits a prompt in a shared notebook "to fix a quick issue"; the change is never reviewed or documented |
| **No rollback** | After a prompt change causes quality degradation, there's no way to quickly revert to the previous state |

### How Automation Helps (These are tools to MANAGE THE ASSET, not "build AI")

| Automation tool | What it does for asset management |
|----------------|-----------------------------------|
| **Prompt registry with version history** | Automatically records every change with a version number, timestamp, and author; enables rollback |
| **Diff views** | Side-by-side comparison of prompt versions to understand what changed and why |
| **Automated dependency tracking** | When a linked data asset or tool spec changes, alert prompt owners to re-validate |
| **Prompt linting/validation** | Check for missing required fields, schema compliance, forbidden content (secrets, PII) |
| **Automated regression checks** | On every version change, run the golden eval set; fail if performance drops below threshold |
| **Protected production labels** | Prevent unauthorized changes to the production-tagged version |
| **Search and discovery API** | Find prompts by task, domain, intent, owner, or status |
| **Webhook triggers** | Notify downstream systems when a prompt version is promoted |

---

## 5. Tool Map: Prompt Management / PromptOps Platforms

| Platform | Vendor | Category | Key Asset-Management Features | Notes |
|----------|--------|----------|-------------------------------|-------|
| **MLflow Prompt Registry** | Linux Foundation | Open source / enterprise | Versioning (immutable sequential versions), commit messages, tags (prompt-level + version-level), aliases (mutable named refs), model_config storage, diff view, search_prompts API, lineage with tracing/evals | Best for data/ML orgs already using MLflow; strong lineage integration ([src-150]) |
| **Langfuse** | Langfuse GmbH | Open source / cloud | Version history (immutable), labels (mutable pointers), diff view, protected labels (admin-controlled), SDK caching, link prompts to traces | Open source; supports self-hosting; strong observability integration ([src-151]) |
| **LangSmith Prompt Hub** | LangChain | Cloud | Commit hashes, commit tags, environment aliases (staging/production), webhook triggers, prompt owner access control, public community hub | Tight integration with LangChain; good for LangChain-based workflows ([src-160]) |
| **Agenta** | Agenta AI | Open source (MIT) | Prompt + Deployment Registry, Git-like branching (variants), version history, comparison mode (50+ models), designed for dev+non-dev collaboration | RBAC, SSO, audit logs in enterprise tier; strong collaboration focus ([src-161]) |
| **PromptLayer** | PromptLayer | Cloud | Out-of-code registry, head-to-head version tests, scheduled regression tests, logging | Easy setup (< 30 min); best for small teams; logging-first ([src-164]) |
| **Maxim AI** | H3 Labs | Cloud | Playground++ (central hub), visual versioning, multi-model comparison, deployment variables, evaluation framework, production monitoring with custom dashboards | Strong eval integration; end-to-end from iteration to monitoring ([src-156]) |
| **LaunchDarkly AI Configs** | LaunchDarkly | Cloud | Runtime prompt updates without redeployment, environment management (dev/staging/prod), A/B testing, gradual rollouts, token usage tracking | Feature flag approach to prompt management; runtime control ([src-155]) |
| **BAML** | BoundaryML | Open source (DSL) | File-based structured prompt format, schema+prompt co-location, Git-diffable, static analysis/linting, validation at parse time, multi-language transpilation | Not a registry — a prompt file format/schema standard; use alongside a registry ([src-158]) |
| **Amazon Bedrock Prompt Management** | AWS | Cloud/enterprise | Version control, evaluation, environment separation (dev/test/prod via CDK/CloudFormation), shadow mode, CI/CD integration guidance | Enterprise-grade; tightly integrated with AWS stack ([src-153]) |
| **Git-based repos** | N/A | Open source | Commit history, diffs, branching, PR reviews, CI/CD hooks | Best as foundation; outgrown by teams with non-engineer contributors; use with a registry on top ([src-162]) |

---

## 6. Operations and Management: Ongoing Governance Activities

### Who Authors, Reviews, and Approves a Prompt Asset

Roles and responsibilities ([src-154], [src-157]):

| Role | Responsibility | When involved |
|------|---------------|---------------|
| **SME (Subject Matter Expert)** | Validates domain logic correctness; confirms judgment criteria | Stage 1 (capture), Stage 7 (review before production promotion) |
| **Prompt Engineer / Data Steward** | Ensures structural correctness, metadata completeness, dependency map, schema compliance | Stages 2–6 |
| **Domain Owner** | Signs off that the prompt is fit for purpose; accountable for production behavior | Stage 7 (approval gate) |
| **Quality Assurance** | Runs evaluation against golden cases; validates regression tests pass | Stage 7 (pre-promotion testing) |
| **Portfolio Governance Council** | Cross-function oversight for enterprise-wide standards | Policy setting; periodic review |

### Refreshing Prompt Metadata When Dependencies Change

When the following happen, a prompt re-validation cycle must be triggered:
- **Data asset version change**: The referenced lookup table, master data, or document was updated
- **Tool/API spec change**: The referenced API endpoint, schema, or behavior changed
- **Glossary term change**: A term used in the prompt was redefined
- **Model version change**: The LLM provider released a new model version that may behave differently

AWS recommends "shadow mode" testing for such changes: run the updated prompt against production traffic without affecting users, observe behavior, confirm expected performance before promoting. ([src-153])

### Periodic Prompt Quality Checks

Recommended cadence ([src-157]):
- **Weekly**: Usage analytics review (which prompts are being used; which are stagnant)
- **Monthly**: Performance review — accuracy, latency, cost trends per prompt version
- **Quarterly**: Full prompt library audit — sample top prompts for relevancy, safety, and cost; rotate reviewers across domains to spot blind spots
- **Triggered**: Any time a dependency changes (immediate re-validation)

### Deprecation and Retirement of Stale Prompts

Deprecation lifecycle ([src-154], [src-157]):
1. Mark prompt status as `deprecated` with deprecation date
2. Notify downstream consumers with a deprecation notice and migration path
3. Maintain availability for a defined window (typically 1 quarter for production prompts)
4. Archive: move out of active registry but retain for audit/historical purposes
5. Remove: delete only after audit retention period expires and all consumers have migrated

### Access Control on Prompt Assets

Role-based access ([src-157], [src-151]):
- **Viewer / Reader**: Can read and use prompt templates; cannot modify
- **Contributor**: Can create new versions; cannot promote to production
- **Domain Owner / Maintainer**: Can review, approve, and promote; can set protected labels
- **Admin**: Can delete prompts, unlock protected labels, manage access

Best practice: "Limit write access for high-risk domains such as legal or healthcare. Use code owners to auto-assign reviewers when a protected area changes." ([src-157])

---

## 7. Common Pitfalls and How to Avoid Them

### Pitfall 1: Prompt Sprawl — No Single Source of Truth

**What it looks like**: 12 copies of "the defect classification prompt" exist in 4 different repos, 3 Slack messages, and a shared spreadsheet. No one knows which is current.

**How to avoid**:
- Establish the prompt registry as the ONLY sanctioned storage location
- Run a one-time discovery scan and migration before announcing the registry
- Block creation of new "unofficial" prompt storage locations through policy

### Pitfall 2: Hidden Dependencies Breaking When Data/Tool Changes

**What it looks like**: The QC standard doc was updated to v3.3, but the prompt still references v3.2 logic. The AI makes the wrong judgment calls—silently.

**How to avoid**:
- Always write a dependency map (Stage 5) before registering a prompt
- Implement automated alerts: when a linked data asset version changes, notify prompt owners
- Run regression checks on the golden eval set any time a dependency changes

### Pitfall 3: Untracked Changes / Copy-Paste Drift

**What it looks like**: Engineers copy a prompt into application code and "slightly improve" it without registering the change. The registry is stale; production uses a different version.

**How to avoid**:
- Enforce that applications ONLY fetch prompts from the registry at runtime (never hardcode)
- Implement automated checks in CI/CD that detect hardcoded prompts in application code
- "Prompt versioning solves this by treating prompts as managed, trackable assets instead of disposable text scattered across the codebase." ([src-162])

### Pitfall 4: Mixing Secrets / PII into Prompt Templates

**What it looks like**: An API key or internal system password is hardcoded into a prompt template. When the template is exported, shared, or logged, the secret leaks.

**How to avoid**:
- Prompt linting rules must flag hardcoded secrets, PII patterns (email, phone, ID numbers), and internal system credentials
- Use variable injection at runtime for any sensitive values
- GitGuardian's 2026 report: 29 million new hardcoded secrets found in public GitHub in 2025 alone, a 34% increase year over year ([src-165])

### Pitfall 5: Skipping Documentation / "Version Numbers Without Context"

**What it looks like**: Registry shows v1.0, v1.1, v1.2 of a prompt, but no commit messages. No one knows why changes were made. When something breaks, no one can tell which change caused it.

**How to avoid**:
- Require a meaningful commit message (change reason) for every version — this is mandatory metadata, not optional
- "Commit messages should explain WHY you changed the prompt, not just what changed." ([src-162])
- Automated enforcement: block promotion if `change_reason` field is empty

### Pitfall 6: Testing Only Happy Paths

**What it looks like**: The prompt passes all test cases on normal inputs. But it silently fails on edge cases (borderline measurements, unusual part numbers, missing fields).

**How to avoid**:
- Golden eval set must include edge cases, boundary conditions, and adversarial inputs
- Run the golden set on every version change before promotion
- Include "known limitations" as a required metadata field — document edge cases explicitly

### Pitfall 7: No Access Control / Anyone Can Overwrite Production

**What it looks like**: An engineer directly edits the production-tagged prompt without review to "quickly fix an issue." The change is never reviewed; it introduces new problems.

**How to avoid**:
- Use protected labels for production versions (Langfuse "Protected Prompt Labels" feature: viewer and member roles cannot modify the production label) ([src-151])
- All production changes must go through the PR/approval workflow
- Audit logs must record who changed the production alias and when

---

## 8. Analogies for Plain-Language Communication

**Analogy 1: "Treat prompts like source code under version control"**
Just as you wouldn't push code to production without version control, testing, and code review — prompts deserve the same treatment. ([src-155], [src-162])

**Analogy 2: "A prompt is a recipe, a harness is the kitchen"**
The prompt (recipe) says what ingredients and steps to use. The harness (kitchen) provides the utensils, the timer, the safety check, and the output packaging. A recipe alone doesn't produce a dish; you need the full kitchen setup.

**Analogy 3: "Prompts as intellectual property, not post-its"**
"A centralized Prompt Library transforms prompts into valuable, reusable corporate intellectual property." ([src-167]) The company's expert judgment—encoded as prompts—is an asset that can depreciate (become stale) and appreciate (improve with refinement) just like other IP.

**Analogy 4: "The assembly line vs. the craftsman"**
Without prompt asset management, every developer reinvents the quality inspection prompt from scratch—craftsman-style. With a governed prompt registry, organizations achieve assembly-line consistency: every call to the QC harness applies the same standard, reliably.

---

## 9. Manufacturing Industry Examples

### Example 1: "Defect Cause Analysis Prompt" Asset
A manufacturing quality team needs an AI to analyze defect reports and identify root causes. The SME's tacit knowledge: "I look at the defect type, the process step where it occurred, the shift data, and the material lot. If it's a dimensional defect at the machining step, I check the tool wear log."

**Asset-ification**:
- Capture: structured interview with quality engineer, recording the decision tree
- Structure: prompt fields include defect_type, process_step, shift_data, material_lot_id, tool_wear_log_flag
- Register: metadata includes owner = quality-engineering-team, model = gpt-4o, version = 1.0.0
- Dependencies: material master database v2, defect type taxonomy v4.1
- Harness: packages the prompt with input validation (requires defect_type + process_step), output schema ({root_cause, confidence, recommended_action}), eval hook (20 golden defect cases)
- Governance: changes require quality lead sign-off; quarterly re-validation when defect taxonomy updates

### Example 2: "Incoming Inspection Judgment Harness"
(See Stage 6 above for full example.)

### Example 3: "C/S Report Drafting Harness"
A customer service team needs to draft formal complaint responses. The SME's procedure: "I look at the complaint category, the customer tier, any previous incidents, and company policy on liability. I draft in formal language with an apology, a cause explanation, and next steps."

**Asset-ification**:
- Prompt fields: complaint_category, customer_tier, previous_incidents (list), policy_ref, output_tone = "formal-apologetic"
- Metadata: owner = cs-team, domain = customer-service, intent = draft, version = 2.0.0
- Dependencies: customer policy document v7, customer tier classification table
- Harness: input validation (complaint required + customer_id), output schema (subject, body, next_steps), eval hook (30 golden complaint responses), fallback (if confidence < 0.6, flag for human review)
- Governance: any template changes require CS director approval; triggered re-validation when policy doc updates

---

## Sources

| ID | Citation |
|----|---------|
| src-150 | MLflow Prompt Registry Documentation. https://mlflow.org/docs/latest/genai/prompt-registry/ (Accessed 2026-06-09) |
| src-151 | Langfuse Open Source Prompt Management. https://langfuse.com/docs/prompt-management/overview (Accessed 2026-06-09) |
| src-152 | Wei Zhang, Jessie Jie Xia. "Structured-Prompt-Driven Development (SPDD)." martinfowler.com, April 28, 2026. https://martinfowler.com/articles/structured-prompt-driven/ (Accessed 2026-06-09) |
| src-153 | AWS Prescriptive Guidance. "Prompt, agent, and model lifecycle management." https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html (Accessed 2026-06-09) |
| src-154 | Segev Shmueli. "Managing Prompts as Enterprise Assets: A Portfolio Approach." Medium, July 20, 2025. https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327 (Accessed 2026-06-09) |
| src-155 | Jesse Sumrak. "Prompt Versioning & Management Guide for Building AI Features." LaunchDarkly Blog, March 28, 2025. https://launchdarkly.com/blog/prompt-versioning-and-management/ (Accessed 2026-06-09) |
| src-156 | Kuldeep Paul. "Prompt Versioning: Best Practices for AI Engineering Teams." Maxim AI, October 15, 2025. https://www.getmaxim.ai/articles/prompt-versioning-best-practices-for-ai-engineering-teams/ (Accessed 2026-06-09) |
| src-157 | Evie Rao. "Reusable Prompt Libraries: Taxonomy and Access." PulseGeek, August 31, 2025. https://pulsegeek.com/articles/how-to-create-reusable-prompt-libraries-taxonomy-metadata-and-access/ (Accessed 2026-06-09) |
| src-158 | BoundaryML. "BAML — The AI framework that adds the engineering to prompt engineering." GitHub/BoundaryML, 2025. https://github.com/BoundaryML/baml (Accessed 2026-06-09) |
| src-159 | Fulcrum Digital. "Prompt Governance: The Emerging Enterprise Control Layer." https://fulcrumdigital.com/blogs/prompt-governance-the-emerging-enterprise-control-layer/ (Accessed 2026-06-09) [JS-required; excerpts from search index] |
| src-160 | LangChain/LangSmith. "Manage prompts." LangSmith Docs. https://docs.langchain.com/langsmith/manage-prompts (Accessed 2026-06-09) |
| src-161 | Agenta AI. "Agenta — Open-source LLMOps platform." https://agenta.ai/ (Accessed 2026-06-09) |
| src-162 | Braintrust. "What is prompt versioning? Best practices for iteration without breaking production." https://www.braintrust.dev/articles/what-is-prompt-versioning (Accessed 2026-06-09) |
| src-163 | Addy Osmani. "Agent Harness Engineering." https://addyosmani.com/blog/agent-harness-engineering/ (Accessed 2026-06-09) |
| src-164 | PromptLayer. "Best Prompt Versioning Tools for LLM Optimization (2025)." https://blog.promptlayer.com/5-best-tools-for-prompt-versioning/ (Accessed 2026-06-09) |
| src-165 | Doppler. "Advanced LLM security: Preventing secret leakage across agents and prompts." https://www.doppler.com/blog/advanced-llm-security (Accessed 2026-06-09) |
| src-166 | Anonymous. "Documenting SME Processes with Conversational AI: From Tacit Knowledge to BPMN." arXiv, December 2025. https://arxiv.org/pdf/2512.05122 (Accessed 2026-06-09) |
| src-167 | ACM SIGMOD 2025. "Prompt Editor: A Taxonomy-driven System for Guided LLM Prompt Development in Enterprise Settings." https://dl.acm.org/doi/10.1145/3722212.3725124 (Accessed 2026-06-09) |
