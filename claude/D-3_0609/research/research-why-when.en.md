# D-3 Prompt/Harness Asset Management — Research: WHY & WHEN
**Topic**: D-3 Prompt/Harness 자산화 (Prompt/Harness Asset Management)
**Cluster**: WHY (concept/structure-first) + WHEN
**Researcher**: Claude Research Agent
**Date**: 2026-06-09
**Source IDs**: src-050 to src-063

---

## 1. What a Prompt/Harness Asset IS — Concept and Structure

### The Basic Idea

A **prompt** is a written instruction that tells an AI system what to do. A **harness** is a higher-order wrapper that bundles a prompt with its context: the input schema it expects, the output format it produces, the data sources or glossary terms it references, and the evaluation criteria used to judge correctness. Together, a prompt/harness encodes two things that previously only lived in a human expert's head:

1. **The work procedure** — the step-by-step method the expert uses to approach a task
2. **The judgment criteria** — the rules, thresholds, and exceptions the expert applies to decide what is correct or acceptable

This is why a prompt/harness is fundamentally different from a script or a query. A SQL query retrieves data; a prompt encodes the *reasoning* and *evaluation logic* applied to that data. When a senior quality inspector writes a prompt that classifies a defect image, the prompt is not just a technical instruction — it *is* the inspector's accumulated judgment, made legible and executable by a machine.

**Prompt/Harness as an asset means**: it has been captured, structured with metadata (version, owner, dependencies, scope), stored in a governed repository, and made available for reuse — just like a data asset in a data catalog. Without that lifecycle discipline, it remains tacit knowledge that happens to be written down. Written down ≠ managed.

### The Composition of a Managed Prompt/Harness Asset

A well-structured prompt/harness asset record typically contains:

| Component | What it captures |
|-----------|-----------------|
| **Prompt text** (versioned) | The actual instruction and reasoning logic |
| **Version ID** | Which exact version produced which output |
| **Owner / Steward** | Who is responsible for keeping it current |
| **Linked data terms** | Which glossary terms, schemas, or data fields the prompt assumes |
| **Linked tools / context** | Which retrieval sources, APIs, or reference data it depends on |
| **Input/output schema** | What it expects in and what it returns |
| **Evaluation criteria** | How "good output" is defined and measured |
| **Deployment environment** | Where it is active (dev / staging / production) |
| **Change log** | Why each version was modified |
| **Deprecation status** | Whether it is still valid or has been superseded |

Without these fields, the prompt is not an asset — it is an untracked text string.

### The Analogy to Data Governance

Prompt governance directly parallels data governance in structure. As Bojan Ciric (Technology Fellow, Deloitte) writes: "Much like data, prompts represent valuable assets that require effective governance to maximize their utility, ensure compliance with organizational policies, and minimize costs." [src-054] The roles mirror exactly: Prompt Stewards correspond to Data Stewards; Prompt Committees to Data Governance Committees; a Prompt Catalog to a Data Catalog. This parallel matters because organizations that already practice data governance can extend — not replace — their existing frameworks to cover prompt assets. [src-054]

---

## 2. WHY — What Goes Wrong Without Prompt/Harness Asset Management

This section focuses on the cause-effect chain: unmanaged prompts → specific organizational failures.

### 2.1 Prompt Sprawl: No Single Source of Truth

When prompts are not managed, they proliferate freely. As Martin Rodek (PromptLedger) observes: "In many organizations, prompt management is handled in an ad hoc manner, with developers crafting and modifying prompts independently across teams and projects. Without a structured system, prompts are frequently stored in disparate locations — ranging from personal notes to scattered repositories." [src-051]

CIO Magazine identifies the pattern explicitly: "Prompt sprawl mirrors earlier patterns of data sprawl, with prompts living everywhere including personal chat threads, shared documents, Slack messages, and wiki pages." [src-059]

The structural problem: there is no authoritative version. A team cannot answer "which version of this defect-classification prompt is currently running in production?" They can only guess.

### 2.2 Copy-Paste Drift: Silent Divergence Across Teams

The most common failure mode is copy-paste proliferation. PE Collective defines it precisely: **Prompt Drift** is "the unintended divergence of production prompts from their registered or intended versions, caused by manual edits, deployment pipeline overwrites, or copy-paste errors across environments. Copy-paste proliferation occurs when someone shares a working prompt in Slack, others modify their local copies, and five slightly different versions end up in production." [src-060]

**Manufacturing example**: A senior quality engineer creates an effective defect-classification prompt for a weld-inspection AI. Colleagues in three other plants copy it to Slack and each modify it slightly. Six months later, four divergent versions are running with no record of what changed or why. When the inspection standard is updated by the quality management team, only one plant's version gets updated — but no one knows which one. The other three continue applying the old criteria silently.

Wipro's analysis (Pravin Shastrakar) frames this structurally: "Prompt Debt: Quick prompt → fragile AI behavior. Common characteristics: Scattered prompts hidden across applications. Duplicate and diverging prompts created via copy–paste reuse. Outdated prompts embedding obsolete policies or assumptions. No version history, ownership, or audit trail." [src-053]

### 2.3 No Reproducibility: Cannot Explain Past AI Outputs

Version control for code has been standard practice for decades. Its core value: any past state can be reproduced. Without version control for prompts, this guarantee disappears for AI outputs.

As CX Today explains: "An accountability record should make it possible to answer: what decision was made, which model version or prompt version was used, what inputs were relied upon, what output was produced, and what controls were in force." [src-061] Without prompt versioning, none of these questions have defensible answers.

**Manufacturing example**: Six months ago, an AI system flagged a batch of parts as conformant. Today, a customer audit questions that decision. The quality team cannot reproduce the AI's analysis because the prompt used at that time no longer exists as a distinct record — it has been overwritten. The result: no traceability, no defensible audit trail, potential regulatory exposure.

Agenta's guide states the principle directly: "Versioned prompts let you recreate specific outputs by using the exact prompt configuration from any point in time." [src-057]

### 2.4 Hidden Dependencies Break Silently When Data, Glossary, or Tools Change

A prompt/harness does not operate in a vacuum. It makes implicit assumptions about the data it consumes: field names, terminology definitions, measurement units, data structures. When those upstream dependencies change without the prompt being updated, the AI continues to run — but produces wrong results with no error message. This is the "silent breakage" problem.

Wipro's scenario illustrates it in enterprise terms: "Scenario 2: Outdated Policy in a Support Bot — A customer support chatbot embeds last year's claims policy in its prompt. The policy changes, but the prompt does not. Impact: Incorrect customer guidance. Compliance escalations. Emergency prompt fixes with no audit trail." [src-053]

In multi-step AI pipelines, the problem cascades. From the search-aggregated evidence on pipeline dependency management: "In multi-step workflows, when the first prompt returns structured output (a JSON schema) that the second prompt consumes, changing the schema in prompt one will break prompt two. These dependencies exist in any multi-step workflow, any agent, any pipeline with more than one LLM call." [src-056/src-057] "Multi-prompt dependencies (chains, agents) create cascading risks where a single prompt change can break downstream steps." [src-057]

**Managing a prompt as an asset requires mapping its dependencies** — explicitly documenting which data terms, schemas, and tools it depends on — so that changes upstream trigger a review of the affected prompts. Without this, every data schema change or glossary update is a potential hidden failure.

### 2.5 Expert Knowledge Loss: Tacit Judgment Leaves with the Person

This failure is the most strategically significant. Prompts and harnesses, when well-crafted, encode the accumulated judgment of expert workers — years of experience distilled into executable instructions. When those prompts are not managed as organizational assets, that knowledge remains personally owned and organizationally ephemeral.

The tacit knowledge problem in manufacturing is structurally identical. Dovient's research (manufacturing knowledge management platform) quantifies the scale: "When a 35-year veteran retires, up to 90% of what makes them invaluable doesn't follow them out the door in a folder of documents. It evaporates." The result: "Manufacturing facilities that have systematized tacit knowledge capture report 15-25% faster problem resolution, 8-12% reduction in quality issues, and 20-30% shorter onboarding timelines for new operators." [src-055]

The parallel to prompt assets is exact. A senior quality inspector who has built an effective defect-judgment prompt over two years of calibration has encoded their tacit inspection criteria in that prompt. If that prompt is stored only in their personal laptop or a shared drive with no version history, ownership record, or formal transfer process, it is not an organizational asset — it is personal knowledge that will disappear when they leave, retire, or move to a different role.

As Springer/Discover AI research on Industry 4.0 tacit knowledge notes: "The conversion of tacit knowledge into organizational knowledge can be promoted through concept maps for visualization, domain ontology for knowledge representation, and logical and semantic reasoning for continuous learning." [src-062] Prompt/harness asset management is the practical mechanism by which this conversion is operationalized.

### 2.6 Cannot Audit Which Instruction Produced Which Output

Regulatory compliance and internal governance increasingly require that organizations be able to explain AI-assisted decisions. This requires knowing not just what model was used, but precisely which version of which prompt was active at the time of a decision.

CX Today's regulatory framing: "Every prompt change must be tracked with metadata, including author, timestamp, and comments, which ensures accountability and supports detailed audit trails... This detailed audit trail is critical for compliance with industry regulations, internal governance policies, and ethical AI guidelines. It allows for quick identification of the source of any AI output, facilitates root cause analysis in case of errors, and provides the necessary documentation for audits and legal inquiries." [src-061]

Without managed prompt assets, the audit trail is broken at the instruction layer — the most human-meaningful layer in the AI stack.

---

## 3. WHY — The Core Transformation: Tacit → Explicit → Managed Organizational Asset

The three-stage transformation at the heart of D-3:

**Stage 1: Tacit knowledge** — Expert knows how to do the task; the procedure and judgment criteria exist only in their mind. Not transferable, not scalable, not auditable.

**Stage 2: Ad-hoc prompt** — Expert writes a prompt. The knowledge is now explicit text. But: it lives in a personal file, has no version, has no owner record, has no dependency map. It is explicit knowledge without governance. Marginally better than Stage 1, but still fragile.

**Stage 3: Managed organizational asset** — The prompt is registered in a governed prompt catalog with: version ID, owner, dependency links, input/output schema, evaluation criteria, change log, and deployment scope. It is now a first-class organizational data asset. It can be reused, reproduced, audited, updated when dependencies change, and transferred when the expert moves on.

The distinction between Stage 2 and Stage 3 is the entire value proposition of D-3. Most organizations currently operate at Stage 2 and believe they have solved the problem because the prompt is "written down somewhere." They have not. Only Stage 3 delivers the organizational properties that make AI results reproducible, auditable, scalable across affiliates, and resistant to knowledge loss.

MLflow's definition of the registry model captures this concisely: "A prompt management system functions as a registry: a centralized, searchable repository where prompts are treated as reusable, auditable assets rather than ephemeral text inputs." [src-063]

---

## 4. Key Objectives (Verbs)

Based on the research synthesis, five core objectives for D-3 Prompt/Harness Asset Management:

1. **Capture tacit expert judgment as a reusable, versioned asset** — Convert human work procedures and decision criteria from personal knowledge into structured, organization-owned records.

2. **Make every AI result reproducible** — Ensure that any past AI output can be recreated by retrieving the exact prompt version, data context, and tool configuration active at the time.

3. **Prevent silent breakage via dependency mapping** — Explicitly link each prompt to the data terms, schemas, and tools it assumes, so upstream changes trigger a prompt review rather than a silent drift.

4. **Govern the instruction layer with ownership and lifecycle discipline** — Assign stewards, define approval workflows, enforce change-log practices, and retire obsolete prompts — treating them with the same rigor as code or data assets.

5. **Enable reuse across teams and affiliates without copy-paste divergence** — Replace informal sharing (Slack, email, copy-paste) with a registry-based mechanism that ensures all users draw from the same governed, current version.

---

## 5. WHEN — Always-Needed vs. Conditional

### Always-Needed (from the moment prompts drive real work)

Prompt/harness asset management is not an advanced-maturity practice. It is needed from the moment prompts move from personal experimentation to shared, consequential use:

- **Multiple people use the same prompt** — As soon as a second person uses a prompt that the first person wrote, version drift becomes a risk. There is no longer a single author who knows the "current" state.
- **AI outputs feed real decisions** — When AI results influence quality calls, scheduling decisions, financial recommendations, or customer interactions, the instruction that produced them must be traceable and versioned.
- **The same prompt is expected to produce consistent results over time** — Even for a single user, if the expectation is "this prompt should work the same way next quarter," versioning is required.

As Maxim AI states: "Prompt management isn't about writing clever instructions; it's about controlling, testing, and understanding how prompts behave across models, versions, and real users, and in 2025, that's no longer optional, it's infrastructure." [src-058]

### Conditional / Lighter Weight

Asset management overhead can be lighter when:
- **Pure one-off experiments** — A prompt used once to explore a question, with no intention to reuse or operationalize. No governance needed until/unless it is promoted to production use.
- **Single-developer, short-horizon prototypes** — A single person building a proof-of-concept where the prompt will be thrown away or completely rebuilt.

The triggering criterion is: *will this prompt be used again, by others, or to produce results that need to be explained?* If yes to any of these, asset management applies.

### Adoption Triggers (When Organizations Cross the Threshold)

From research synthesis across sources [src-050, src-051, src-052, src-053]:

| Trigger | Why it forces asset management |
|---------|-------------------------------|
| Multiple people editing the same prompt | Copy-paste drift risk is immediate |
| AI outputs feeding real business decisions | Traceability and reproducibility become mandatory |
| Need to reproduce or audit a past AI result | Requires point-in-time version retrieval |
| Scaling a successful prompt to other affiliates | Single source of truth needed; informal copy will diverge |
| Regulatory or compliance audit requirement | Audit trail of prompt versions is legally necessary |
| Expert who built the prompt may leave | Knowledge transfer requires ownership record and versioned text |
| Prompt depends on data terms/schemas that can change | Dependency mapping needed to prevent silent breakage |

Dataversity/PromptOps research puts quantitative context on adoption pace: weekly generative AI usage in enterprises grew from 37% to 72% in one year (2023–2024), with a 130% increase in spending. [src-050] This pace means most organizations are crossing these thresholds right now, not in a future planning horizon.

---

## 6. Manufacturing Context: The Quality Inspection Scenario

To make the abstract concrete for the target audience (manufacturing / industrial enterprise):

**The scenario**: A senior inspection engineer at a manufacturing plant develops an AI-powered defect-classification workflow. She writes a prompt that encodes her 15 years of visual inspection judgment: how to describe ambiguous surface defects, which defect types to flag vs. accept at various tolerance levels, and how to handle edge cases the formal procedure does not cover.

**Without asset management**:
- The prompt lives in her laptop, shared informally to a colleague via email.
- The colleague modifies it slightly for a different product line. Now there are two versions with no record of which changed what.
- Six months later, the quality standard is updated. She updates her prompt. The colleague's version is not updated — he doesn't know the standard changed.
- When a customer audit occurs, the plant cannot answer: "Which prompt version was used to accept batch #4471? Does it reflect the current standard?"
- When she retires, her calibrated judgment — embedded in the prompt — disappears from the organization.

**With asset management**:
- The prompt is registered in the prompt catalog with version v1.0, ownership record, and a link to the quality standard it references.
- When the colleague adapts it, he creates a linked variant (v1.0-ProductLineB), not an independent copy.
- When the quality standard is updated, the catalog flags all prompts that reference the old standard for review.
- The audit can retrieve the exact prompt text, version, and owner active for batch #4471 on the date it was processed.
- When she retires, the organization owns a versioned, documented asset — not a personal file.

This is the organizational value of D-3.

---

## Sources

| Source ID | URL | Title |
|-----------|-----|-------|
| src-050 | https://www.dataversity.net/articles/prompt-engineering-is-dead-long-live-promptops/ | Prompt Engineering Is Dead – Long Live PromptOps (Dataversity) |
| src-051 | https://medium.com/@martin_rodek/why-large-enterprises-need-a-prompt-registry-for-ai-governance-04d744039bb4 | Why Large Enterprises Need a Prompt Registry for AI Governance |
| src-052 | https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327 | Managing Prompts as Enterprise Assets: A Portfolio Approach |
| src-053 | https://wiprotechblogs.medium.com/managing-prompt-technical-debt-in-enterprise-ai-7985f4bc7ff7 | Managing Prompt Technical Debt in Enterprise AI (Wipro) |
| src-054 | https://medium.com/the-future-of-data/prompt-governance-enabling-reusability-and-standards-in-ai-driven-organizations-1d4dfe0a83ea | Prompt Governance: Enabling Reusability and Standards (Deloitte Fellow) |
| src-055 | https://dovient.com/resources/blog/tacit-knowledge-manufacturing-hidden-asset | Tacit Knowledge in Manufacturing: Capture It Before It's Gone |
| src-056 | https://www.prompts.ai/blog/decoupled-ai-pipelines-dependency-management-best-practices | Decoupled AI Pipelines: Dependency Management Best Practices [BLOCKED] |
| src-057 | https://agenta.ai/blog/prompt-versioning-guide | Prompt Versioning: The Complete Guide (Agenta) |
| src-058 | https://www.getmaxim.ai/articles/prompt-versioning-best-practices-for-ai-engineering-teams/ | Prompt Versioning Best Practices for AI Engineering Teams (Maxim AI) |
| src-059 | https://www.cio.com/article/4130960/prompt-governance-is-the-new-data-governance.html | Prompt Governance Is the New Data Governance (CIO) |
| src-060 | https://pecollective.com/glossary/prompt-drift/ | What Is Prompt Drift? Definition & Examples |
| src-061 | https://www.cxtoday.com/security-privacy-compliance/ai-audit-trail-regulatory-scrutiny/ | How to Build AI Audit Trails That Stand Up to Regulatory Scrutiny |
| src-062 | https://link.springer.com/article/10.1007/s44163-022-00020-w | Tacit Knowledge Elicitation Process for Industry 4.0 (Springer) |
| src-063 | https://mlflow.org/prompt-registry | Prompt Registry for LLMs & Agents (MLflow) |
