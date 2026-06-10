# D-3 Prompt/Harness Asset-ization — How (Core) Research Notes

**Cluster:** How — methodology / process / tooling for ASSET-IZING prompts & harnesses  
**Date:** 2026-06-10  
**Language:** English (first-language research for later translation)

---

## 1. How to Capture & Convert Human Work Procedures into Structured Prompts

### 1.1 The Conversion Challenge: Tacit to Explicit

The deepest challenge in prompt asset-ization is not tool selection — it is knowledge extraction. Senior workers carry judgment, not just procedures. A quality engineer inspecting automotive parts does not consult a checklist; she pattern-matches against years of failure modes. That pattern-recognition must become explicit, measurable criteria for an AI to execute reliably.

MartinFowler.com (Rahul Garg, Thoughtworks, 2026-03-31) frames this precisely: "The act of writing [a structured instruction] surfaces assumptions that were never articulated. What *exactly* makes a security issue critical versus merely important? These distinctions, often intuitive for the senior, must become precise for the AI." [src-205]

### 1.2 The SME Extraction Interview

The method that works in practice is a structured interview with domain experts, organized around pointed questions:

- What decisions should never be left to individual judgment?
- Which criteria are applied instinctively by experienced staff but rarely written down?
- What triggers an immediate escalation or rejection?
- What separates a satisfactory output from a problematic one?
- Which mistakes recur in the work of less experienced staff?

The answers map directly to prompt components: non-negotiable standards become hard constraints; frequent corrections become explicit check items; experienced instincts become priority-ranked judgment criteria. [src-205]

**Key insight from practice:** The extraction process itself has value beyond the prompt. On one project, the extraction conversation revealed that two senior engineers had "quietly different thresholds for what counted as a 'critical' security concern versus an 'important' one, a disagreement that had never surfaced because each reviewed different pull requests. The act of writing a shared instruction forced the distinction to be made explicit." [src-205]

The CARE (Collaborative Agent Reasoning Engineering) framework (arXiv, 2025) formalizes this with helper agents generating targeted, phase-aligned elicitation questions and drafting initial artifact sets (constraints, decomposition targets, intended users) to seed SME validation. [src-210]

### 1.3 SOP-to-Prompt Decomposition: The PTCF Structure

The industry-standard structure for SOP conversion uses four components (PTCF): [src-210]

1. **Persona**: The expertise level and perspective the AI should adopt (e.g., "You are a quality assurance engineer specializing in surface defect inspection for injection-molded parts")
2. **Task**: The exact action to perform (e.g., "Classify each flagged item using the defect severity table")
3. **Context**: Real-world operational grounding (reference standard version, batch number, product line)
4. **Format**: Output structure that matches downstream process needs (e.g., JSON report with mandatory fields)

A well-structured executable instruction has four additional elements critical for maintainability [src-205]:

1. **Role definition** — not for persona, but to set the expertise frame and scope
2. **Context requirements** — what data/references the instruction needs before it can operate; makes dependencies explicit
3. **Categorized standards with priority tiers** — "must follow / should follow / nice to have" or "critical / important / advisory." This priority structure encodes *judgment*, not just knowledge.
4. **Output format** — structured enough to be comparable across runs and operators

### 1.4 Manufacturing Example: Quality Inspection SOP → Prompt

**Before (vague SOP):** "Inspect for defects"

**After (explicit prompt-ready specification):**
- Role: "You are a quality control inspector with expertise in [product type] surface inspection per standard VS-002"
- Input contract: {product_id, batch_number, reference_standard_version, inspector_id}
- Judgment steps (enumerated with measurable thresholds):
  - Scratches: flag if > 5mm length
  - Cracks: flag any size
  - Discoloration: flag if > 10% of surface area
- Decision rules (if-then): Severity levels Critical / Major / Minor with explicit cutoffs
- Reference criteria: Link to defect classification guide VS-002 as few-shot examples for borderline cases
- Prohibited actions: "Do not pass ambiguous cases without escalation to supervisor"
- Output format: JSON `{defect_type, severity, location_on_part, measurement_mm, action_required, reference_standard_section}`

[src-210, src-205]

### 1.5 Tacit Knowledge Documentation Techniques

From knowledge management literature, additional methods for surfacing tacit criteria [src-210]:
- **Think-aloud protocols**: Have experts narrate their reasoning while performing the task
- **Storytelling elicitation**: Ask about memorable edge cases and how they were resolved
- **Decision table construction**: Enumerate condition combinations and corresponding actions
- **Failure-mode review**: Walk through what has gone wrong in the past and what the correct response was
- **Contrast analysis**: Compare a "good output" and a "borderline output" side by side; the explanations reveal the criteria

---

## 2. Prompt Structuring Best Practices for Maintainable Assets

### 2.1 The Four-Layer Separation

Effective, maintainable prompts separate four concerns: [src-205, src-203]

1. **Role / Persona**: Who is the AI acting as? Sets expertise frame and perspective filter.
2. **Instructions / Logic**: The categorized steps and standards — what to check, in what order, with what priority tiers.
3. **Context / Data contract**: What variable inputs are injected at runtime (e.g., `{{product_id}}`, `{{batch_number}}`, `{{customer_name}}`)?
4. **Output format / Schema**: The exact structure the downstream system expects — ideally a JSON schema.

This separation makes each component independently modifiable: a persona update does not require touching the logic; a schema change does not require re-eliciting knowledge.

### 2.2 Template Variables and Composable Modules

Prompts should use named template variables (`{{variable_name}}`) for all runtime-variable content. This enables: [src-201, src-202]
- Parameterized reuse across teams and contexts (different product lines, different customers, different language locales)
- Clean separation of static instruction logic from dynamic data
- Testability — you can run regression tests by substituting known test values

Langfuse and MLflow both support this; Langfuse extends it with **prompt composability** — the ability to embed one versioned prompt inside another using a reference syntax (`@@@langfusePrompt:name=PromptName|label=production@@@`). This enables shared components (e.g., a company-specific tone guide, a standard disclaimer block) to be maintained once and referenced by many prompts. [src-202]

### 2.3 Output Schema as Part of the Asset

The output format is not secondary. A versioned prompt should include its expected output schema (JSON schema preferred) as a first-class component. Reasons: [src-204, src-203]
- Output format changes break downstream systems — they must be tracked as breaking changes (MAJOR version bump)
- JSON schema enables automated structural validation in CI/CD pipelines ("deterministic checks run first and catch structural failures such as missing required fields, invalid JSON")
- Schema documentation enables dependency mapping (which downstream systems parse this output?)

### 2.4 Style Guides and Prompt Templates

For organizations with many prompts, a team-level style guide establishes conventions:
- How to name prompts (domain + process + function, e.g., `quality.inspection.defect-classifier`)
- Standard section ordering (role → context → instructions → output)
- Tone and persona vocabulary for customer-facing vs. internal prompts
- Maximum instruction complexity thresholds (when to split into modular sub-prompts)
- Mandatory metadata fields in every prompt header

[src-203, src-205]

---

## 3. Version Management

### 3.1 The Immutability Principle (Most Important Rule)

"Once a prompt version is published to production, it must never be modified. Any change — even a typo fix — creates a new version." [src-204]

Prompts must be treated as **immutable artifacts**, not configuration strings. The mental model: creating a new prompt version is like cutting a software release — it gets a new identifier; the old version remains intact and accessible; rollback means changing a pointer, not editing text.

The reason this matters: without immutability, you cannot answer "what prompt was running when that incident happened?" Your entire observability stack is undermined if trace IDs cannot be pinned to exact prompt text.

### 3.2 Semantic Versioning (SemVer) for Prompts

SemVer (MAJOR.MINOR.PATCH) translates directly to prompts [src-204, src-203]:

| Version bump | Prompt trigger examples | Why it matters |
|---|---|---|
| **MAJOR** (X.0.0) | Output format change, role/persona rewrite, model switch, logic restructuring | Breaks downstream parsers or dependent processes; consumers need advance notice |
| **MINOR** (x.Y.0) | New judgment criteria added, additional few-shot examples, new output field (backward compatible) | New capability without breaking existing behavior |
| **PATCH** (x.y.Z) | Typo fix, minor wording clarity, formatting whitespace | Should not materially alter model behavior |

**Critical rule:** "If a patch bump causes a measurable behavior change in your evaluation suite, it should have been a minor or major bump." [src-204]

**What gets versioned together** (the version is meaningless without all of these): [src-204]
- Prompt template text
- Model name AND model version (e.g., `claude-sonnet-4-6`, not just `claude-sonnet`)
- Temperature and sampling parameters
- Retrieval / RAG configuration (if applicable)
- Tool/function definitions (if applicable)
- Author, change rationale, approver, timestamp

### 3.3 Prompt Registry Setup

A prompt registry is the central store for all versioned prompt assets. Minimum viable registry provides: [src-202, src-203, src-201]

- **Storage**: Immutable versioned artifacts (text + metadata)
- **Labeling / tagging**: Environments (draft, staging, production), experiments, tenant variants
- **Fetch API**: Retrieve by version ID or label (e.g., "give me the production version of `quality.inspection.defect-classifier`")
- **Diff view**: Side-by-side comparison of any two versions
- **Audit log**: Who changed what, when, why, with approver

Langfuse's label system is the reference implementation: labels (production, staging, latest) are human-readable pointers to version IDs; they float independently of version numbers; rollback = reassigning the `production` label to a previous version. [src-201]

### 3.4 Environments: Draft → Staging → Production Pipeline

Standard environment progression: [src-204, src-203]

```
Draft (author edits freely)
  → Staging (SME/reviewer validates behavior)
    → [Automated eval gate: no regression > 3% on golden dataset]
      → Production (immutable; change requires new version)
```

Protected labels (Langfuse Enterprise) prevent the `production` label from being reassigned by non-admin roles — an important guardrail in regulated environments. [src-201]

### 3.5 Approval Workflow Before High-Impact Deployment

Drawing on the Humanloop model (influential before its 2025 acquisition by Anthropic) and current best practices [src-209, src-204]:

1. Author creates new version in draft
2. Automated structural validation (schema check, variable binding check)
3. SME/domain reviewer leaves comments and approves behavioral intent
4. AI platform team reviews for technical risk
5. Automated eval gate: run against golden dataset; block if quality drops below threshold
6. Compliance reviewer (in regulated industries) signs off
7. Promotion to production via label assignment (no code deploy required)

The key tension: "Product teams want fast iteration. Platform teams want guardrails. The resolution: non-technical stakeholders can author and iterate prompts through a GUI, but promotion to production requires passing automated evaluation gates that the engineering team owns." [src-204]

---

## 4. Dependency Management

### 4.1 Dependency Mapping for Every Prompt Asset

Each prompt asset should carry an explicit dependency manifest: [src-204, src-203]

```yaml
prompt_id: quality.inspection.defect-classifier
version: 2.1.0
dependencies:
  model: claude-sonnet-4-6   # pinned, not floating
  data_assets:
    - defect_taxonomy_v3     # ontology/glossary term set
    - reference_images_VS-002  # few-shot examples
  tools: []
  eval_set: defect-classifier-golden-v2
  downstream_consumers:
    - quality-dashboard-api (parses output JSON field: severity)
    - capa-workflow-trigger (reads action_required field)
```

### 4.2 Impact Analysis When a Dependency Changes

When any dependency changes, the prompt registry should trigger impact analysis [src-204, src-202]:

- **Model upgrade**: Re-run full eval suite; treat as potential MAJOR version trigger. "Pin the model snapshot version (e.g. `claude-sonnet-4-6-20251014`, not `claude-sonnet-4-6`) and log it on every call. When quality drops, check whether `prompt_version` or `model_version` changed since the last good window."
- **Glossary/taxonomy term renamed or redefined**: Identify all prompts that reference the term; flag for review; prompt owner must validate judgment criteria still hold
- **Tool schema changed** (e.g., inspection database field renamed): Prompts using that tool must be updated; output schema may need to change (MAJOR bump)
- **Few-shot examples updated**: Minor bump if behavioral intent unchanged; major if examples fundamentally shift the judgment calibration

### 4.3 Silent Regression from Model Provider Updates

Providers update model weights without notice. A February 2026 longitudinal study confirmed "meaningful behavioral drift across deployed transformer services" over a ten-week period, with attribution impossible because providers don't release update logs. [src-204]

Defense: daily automated evaluation runs against the golden dataset, comparing against baseline scores. Drift above threshold triggers alert and blocks new deployments.

---

## 5. Degradation Detection and Improvement Loop (Light Touch)

### 5.1 Golden Dataset

A curated, versioned collection of representative inputs (typical cases + edge cases + known failure modes) plus their expected outputs. This is the primary regression-detection mechanism: [src-204]

- Run on every prompt deployment
- Run on a daily schedule to catch provider-side model drift
- Block deploys when overall quality score drops more than 3% relative to main branch baseline
- Add every production failure case to the dataset
- Sample 1% of real traffic into evaluation queue continuously

### 5.2 User-Edit Rate as an Improvement Signal

If users frequently override or edit AI outputs from a particular prompt, that edit rate is a leading indicator of prompt degradation. Track:
- Edit rate per prompt (how often users modify the output)
- Override categories (what type of changes users make most often)
- Escalation rate (how often the AI output triggers human review)

These signals should feed back to the prompt owner as improvement priorities. [src-203, src-302 (cross-ref)]

### 5.3 LLM-as-Judge for Open-Ended Outputs

For outputs that cannot be validated with pattern matching (summaries, reasoning, judgment calls), LLM-as-Judge is the 2025 standard: a strong model evaluates whether another model's output meets quality criteria. Scales to thousands of evaluations overnight. [src-204]

---

## 6. Harness Packaging

### 6.1 The Harness Concept: Why Prompts Alone Are Insufficient

By 2025-2026, the industry recognized that a prompt divorced from its execution context is an incomplete asset. "The key metric in 2026 isn't prompt quality — it's KV-cache hit rate and harness complexity." [src-208]

The evolution: Prompt Engineering (2022-2024) → Context Engineering (2025) → Harness Engineering (2026).

"In 2022, we studied how to write the perfect email. In 2025, we learned to manage our inbox. In 2026, we're designing the email system itself." [src-208]

Why prompts alone failed: "A team spends three weeks polishing their coding agent's prompt... Then the project scales up, and things break... No matter how many times you write 'reuse existing code' in the prompt, if the utility file isn't in the context window, the agent doesn't know it exists. The prompt was perfect, but the information the agent could see was incomplete. This isn't a prompt problem. It's a context problem." [src-208]

### 6.2 Harness Components (What to Bundle)

A harness packages a complete, reusable, repeatable work unit. A minimal harness record: [src-208, src-206]

| Component | Description | Example |
|---|---|---|
| Prompt(s) | Versioned instruction set(s) | `quality.inspection.defect-classifier@v2.1.0` |
| Input data contract | Required fields, types, sources | `{product_id: str, batch_no: str, image_ref: url}` |
| Tool-call order | Which tools are called, in what sequence | Search defect taxonomy → call vision API → lookup reference images |
| Model + parameters | Pinned model version and temperature | `claude-sonnet-4-6-20251014, temp=0.1` |
| Output format | Expected schema of the final output | JSON schema v2.1 |
| Eval set link | Reference to the golden dataset for this harness | `defect-classifier-golden-v2` |
| Documentation | Purpose, inputs, outputs, owner, version history | Standard template |

The Harness.io Worker Agent pattern is a reference implementation: "Each Worker Agent pairs a prompt (Instructions), a Model Connector, and optional MCP Servers into a single, reusable, governed step." [src-208]

### 6.3 Harness Registry

Harnesses, like prompts, need a registry with:
- Parameterization for reuse (variables for team/affiliate/locale customization)
- Versioning (harness version ≠ prompt version — a harness version captures the full bundle)
- Discovery (search by domain, process, function)
- Documentation standards (mandatory fields: purpose, input contract, output schema, owner, dependencies, eval set)
- Deprecation management (lifecycle: active → deprecated → retired)

### 6.4 Parameterization for Cross-Team / Cross-Affiliate Reuse

A harness built for one team's inspection process can be adapted for another affiliate's use by parameterizing:
- The defect taxonomy reference (different product families use different standards)
- The output language (Korean / English / Japanese)
- Severity threshold overrides (different customer contracts may require different standards)
- Model selection (some affiliates may have different API access)

The harness registry stores the base harness; affiliates register their parameterized variants under a namespaced identifier (`quality.inspection.defect-classifier.affiliate-A-v1`).

---

## 7. Operations & Governance

### 7.1 Roles and Responsibilities

The org model that has emerged across mature implementations [src-204, src-203, src-205]:

| Role | Responsibilities |
|---|---|
| **Prompt Owner / Domain Owner** (Product lead, SME) | Define behavioral requirements and success criteria; approve changes to prompt intent; subject matter validation |
| **Prompt Engineer** (AI/ML engineer) | Technical implementation, optimization, testing, maintenance; owns evaluation methodology; reviews for technical risk |
| **Compliance Reviewer** | Signs off on prompts in regulated contexts before production promotion (required for finance, healthcare, legal) |
| **Central AI/Data Org** | Owns the registry infrastructure; sets standards and style guides; manages the golden dataset; handles cross-team reuse |
| **Release Coordinator** | Manages deployment cadence; monitors rollouts; coordinates incident response |

"Prompts sit at the intersection of product intent, legal interpretation, and technical execution, which means no single existing role owns them naturally." [src-204]

### 7.2 Review Cadence

- **Every change**: Automated structural validation + eval gate
- **Monthly**: Prompt owner reviews usage analytics, edit rates, escalation rates
- **Quarterly**: Full portfolio review — identify underperforming, unused, or redundant prompts
- **Triggered**: Any model provider update triggers a re-evaluation run across all production prompts

### 7.3 Retirement and Deprecation

Prompts should have explicit lifecycle states: Active → Deprecated (still running but no new development) → Retired (removed from production, archived).

Deprecation requires:
- Notification to downstream consumers
- Migration period (old version still accessible in staging/archive)
- Documentation of why it was retired and what replaced it

Failing to retire old prompts leads to prompt sprawl — the most common governance failure. [src-207]

### 7.4 Audit Trail Requirements

Every prompt version record must capture: [src-207, src-204]
- Author (who wrote it)
- Approver (who authorized production deployment)
- Timestamp (when each version was created/deployed/retired)
- Rationale (why the change was made — free text mandatory)
- Evaluation results at time of deployment (pass/fail + scores)
- Production period (when this version was active in production)

The EU AI Act (January 2025 implementation) requires documented governance for business-critical AI prompts. IBM research (2025) found 68% of commonly used prompt templates unintentionally introduce demographic bias — audit trails are the mechanism by which bias can be detected, attributed, and corrected. [src-207]

---

## 8. Tool Map: Prompt Management & Harness Solutions

### 8.1 Comparison Table

| Solution | Vendor | Category | Key Features | Self-hostable | Pricing model | Notes | Source |
|---|---|---|---|---|---|---|---|
| **Langfuse** | Langfuse GmbH (YC) | Open-source LLMOps | Versioning (auto + labels), RBAC, composability, diff view, playground, eval, caching, GitHub integration | Yes (easy — Terraform/Helm) | Free OSS; Cloud plans; Enterprise EE | Recommended #1 by Nearform (Oct 2025); MIT license; 16.9k GitHub stars | src-201, src-202 |
| **MLflow Prompt Registry** | Databricks / Linux Foundation | MLOps platform | Auto versioning, aliases, eval (Databricks-managed has more features), Agent-as-Judge scorer | Yes (but no official guide) | Free OSS; Databricks managed paid | Good if already using MLflow; many eval features behind managed version; Apache-2.0; 22.4k GitHub stars | src-202 |
| **Amazon Bedrock Prompt Management** | AWS | Cloud platform | 3-way side-by-side variant testing, Prompt Optimization (auto-rewrite), versioning, team collaboration via SageMaker Unified Studio | No (AWS-managed only) | AWS usage-based | GovCloud available; integrates with Bedrock Flows & Agents; strong enterprise compliance | src-206 |
| **Azure AI Foundry (Prompt Flow)** | Microsoft | Cloud platform | Visual pipeline orchestration, model catalog (1800+), eval pipelines, variant testing | No (Azure-managed) | Azure usage-based | Cannot version individual prompts — only entire LLM applications; vendor lock-in; low activity (last release Jan 2025) | src-202 |
| **LangSmith** | LangChain | Closed-source SaaS | Prompt Playground, versioning via commits/tags, multi-provider config, multimodal, eval | No | Paid SaaS | Tightly coupled to LangChain ecosystem; vendor lock-in; no self-hosting | src-202 |
| **PromptLayer** | PromptLayer | Prompt registry SaaS | Visual prompt management, release labels, eval pipelines, logging/analytics | Enterprise license required | Free tier; paid | Simple and low-friction; started as logging layer | src-202, src-203 |
| **Humanloop** | Anthropic (acquired Aug 2025) | Enterprise LLMOps | Review workflows, approval gates, Git/CI-CD integration, RBAC, eval, feedback loops | N/A | N/A — sunset Sep 8, 2025 | Influential governance model; patterns now adopted by others | src-209 |
| **Agenta** | Agenta AI | Open-core LLMOps | All-in-one; observability, eval | Yes (OSS) | Free OSS; paid cloud | Cannot version individual prompts — only application variants | src-202 |
| **Arize Phoenix** | Arize AI | Open-source observability | Prompt versioning, registry, user management, evals, self-hostable | Yes | Free OSS | Client libraries at early stage; good potential | src-202 |
| **Git-based** | N/A | DIY / Infrastructure | Version control, PR review, CI/CD integration, full audit trail | N/A (own infrastructure) | Free (infra cost) | Combined with eval framework; lowest vendor lock-in; requires custom tooling | src-204, src-205 |
| **LaunchDarkly** | LaunchDarkly | Feature flag platform | Prompt versioning via feature flags, canary rollout, instant rollback, A/B testing | No | Paid SaaS | Decouples prompt deployment from code deploy; prompt changes via flag flip | src-204 |

### 8.2 Selection Guidance

- **Start here (general):** Langfuse — open-source, most complete feature set, self-hostable, not locked in
- **AWS shops:** Amazon Bedrock Prompt Management — native integration, GovCloud available
- **Already on Databricks:** MLflow Prompt Registry — avoid adding another platform
- **Git-first teams:** Git + Langfuse for registry; use LaunchDarkly or feature flags for controlled rollout
- **Regulated industries:** Langfuse Enterprise (protected labels) or Bedrock (GovCloud); ensure audit trail and RBAC

---

## 9. Pitfalls

### 9.1 The Top 7 Failure Modes

**1. Prompt Sprawl** (most common)
Prompts live in personal chat histories, shared documents, Slack messages, wiki pages, and email threads. No central registry, no versioning, no owner. By the time leadership notices, the organization has hundreds of conflicting, redundant, and untested prompts. "Teams are using GenAI daily, often productively but prompts live everywhere." [src-207]

**2. Copy-Paste Divergence**
Team A copies Team B's prompt for a similar task, modifies it slightly, and the two copies immediately diverge. Within months, there are 12 variants of what should be one canonical prompt, each with different edge-case handling. No mechanism to propagate improvements back to the shared source.

**3. Prompts Hard-Coded in Application Code**
The single most technically damaging pitfall. When prompts are strings embedded in source code, every change requires a code deploy; iteration is slow; non-technical domain owners cannot participate; version history is buried in git commits mixed with unrelated code changes; and there is no way to rollback the prompt independently of the application.

**4. No Owner, No Accountability**
"A common postmortem finding: engineers can't identify who made the last prompt change or why, because it happened in a DM conversation, was applied directly in a GUI, and was never documented anywhere." [src-204] When there is no designated prompt owner, prompts drift without accountability.

**5. Untested Changes Breaking Workflows**
"A team added three words to a customer service prompt to make it 'more conversational.' Within hours, structured-output error rates spiked and a revenue-generating pipeline stalled." [src-204] Without automated eval gates, even tiny changes can cause cascading failures.

**6. Silent Degradation from Model Updates**
Without a golden dataset and daily evaluation runs, organizations discover that prompts have been producing degraded outputs for weeks or months before anyone notices. Model providers push weight updates without announcement. This is not hypothetical — it was documented at scale in 2025. [src-204]

**7. Prompt Template Bias Accumulation**
IBM research (2025): 68% of commonly used enterprise prompt templates unintentionally introduce demographic bias. Without governance review, biased judgment criteria calcify into production. [src-207]

### 9.2 Organizational Anti-Patterns

- **"Prompts are just config"**: The mental model that prompts are cheap configuration leads organizations to skip all the process infrastructure (versioning, review, testing) that any other production artifact would receive.
- **Governance theater**: Creating a prompt registry that nobody uses because the workflow is too slow; engineers bypass it and continue embedding prompts in code.
- **Documentation graveyard**: Prompt standards written once, never maintained; they drift from actual practice within months because they are not in the workflow.
- **One-size-fits-all governance**: Applying the same heavyweight review process to every prompt creates bottlenecks; applying no process to any prompt is negligence. The answer is tiered governance based on business impact and risk classification.

---

## Sources

| ID | Title | URL |
|---|---|---|
| src-201 | Langfuse Version Control Docs | https://langfuse.com/docs/prompt-management/features/prompt-version-control |
| src-202 | Nearform: Prompt Management Systems Compared | https://nearform.com/digital-community/prompt-management-systems-compared/ |
| src-203 | Segev Shmueli: Managing Prompts as Enterprise Assets | https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327 |
| src-204 | Tian Pan: Prompt Versioning and Change Management in Production | https://tianpan.co/blog/2026-03-13-prompt-versioning-change-management-production |
| src-205 | Rahul Garg (Thoughtworks/MartinFowler.com): Encoding Team Standards | https://martinfowler.com/articles/reduce-friction-ai/encoding-team-standards.html |
| src-206 | AWS: Amazon Bedrock Prompt Management | https://aws.amazon.com/bedrock/prompt-management/ |
| src-207 | CFA Tech / AISquare: Prompt Sprawl to Governance | https://cfatech.ng/from-prompt-sprawl-to-governance-bringing-order-to-chaos |
| src-208 | Jonas Kim: From Prompts to Harnesses — Four Years of AI Agentic Patterns | https://bits-bytes-nn.github.io/insights/agentic-ai/2026/04/05/evolution-of-ai-agentic-patterns-en.html |
| src-209 | Humanloop: Prompt Management (historical) | https://humanloop.com/platform/prompt-management |
| src-210 | Klariti / arXiv CARE: SOP-to-Prompt Conversion | https://klariti.com/2025/04/14/getting-started-guide-to-prompt-engineering-for-sops/ |
