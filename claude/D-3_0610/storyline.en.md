# D-3 Prompt/Harness Asset-ization — AI-Ready Data Guide Storyline (English working draft)

> Source for slides. **Main slides = the correct storyline built from what's needed to understand.** `[Slide N]`
> **Backup slides = detail you drill into from a main when needed.** `[Backup N-x]` (directly under that main)
> Every main slide includes ① one-line summary ② inline term gloss ③ manufacturing field example.
> Topic = "preparing data FOR AI": here, the data being prepared is the **instruction & judgment logic itself** —
> human work procedures and judgment criteria, structured and managed as AI-executable assets.
> Scope guard: tool specifications = D-2 / evaluation datasets = E-3 / feedback-loop operations = E-4.

---

## 1. Executive Summary

### [Slide 1] One-slide summary

**👉 One-line summary:** When people start working with AI, the know-how of "how we do this job" moves into prompts — and unless those prompts are managed like official documents, the company's work knowledge ends up scattered, inconsistent, and lost.

**Headline:** Prompt/Harness asset-ization turns each person's AI work instructions into versioned, reusable, auditable organizational assets — the SOP library of the AI era.

- **Definition:** A system for structuring human work procedures and judgment criteria into AI-executable **prompts** (instruction text given to AI) and **harnesses** (a prompt packaged with its input data contract, tool-call order, and output format into a repeatable work unit), and managing them with version control, ownership, and dependency tracking.
- **Why now:** Every AI task already runs on a prompt somebody wrote. Today those prompts live in personal chat histories and copy-pasted files — unversioned, unowned, irreproducible. The knowledge is being created anyway; the only question is whether it accumulates as a company asset or evaporates. [src-304, src-301]
- **Key build approach (4 keywords):** ① **Select** the prompts worth managing (repeated, judgment-bearing) ② **Structure** expert procedures into prompt form (role·criteria·output format·prohibitions) ③ **Register & version** in a prompt registry with approval and dependency mapping ④ **Package** as reusable harnesses for cross-team/affiliate reuse.
- **Headline benefits (2-3):** Same task → same quality answer regardless of who runs it; AI behavior changes become traceable and reversible (audit-ready); new AI tasks start from the library, not from zero.
- **One-line roadmap:** Inventory & registry first (short) → versioning·approval·regression gates (mid) → harness packaging and group-wide reuse (long).

---

## 2. Why? — why it is needed (concept/structure-first)

### [Slide 2] A new kind of data asset appears the moment work moves to AI

**👉 One-line summary:** A prompt is not a chat message — it is the company's work procedure and judgment criteria written down in a form AI can execute, which makes it a data asset.

**Headline:** When a person hands a task to AI, the "how" of that task — steps, criteria, exceptions, output form — moves out of their head and into a prompt. That text IS organizational knowledge.

- Structure of the idea, in plain terms: any work instruction to an AI contains four things —
  ① **the role** ("act as a quality inspector"), ② **the procedure** ("check A, then B, then C"),
  ③ **the judgment criteria** ("flag a scratch over 5mm; never pass an ambiguous case"), ④ **the output form** ("return a table with these columns").
  These are exactly the contents of an SOP (Standard Operating Procedure = the official, versioned document that defines how a job is done). The prompt is an SOP that a machine can execute. [src-205, src-307]
- Therefore the same logic that made companies manage SOPs as controlled documents applies to prompts: if the procedure matters, the document that encodes it must be findable, versioned, approved, and owned. "Prompts are organizational knowledge artifacts, not just API inputs." [src-305, src-304]
- And a prompt rarely works alone: it runs together with input data, tools it may call, and an expected output format. That complete bundle is the **harness** (everything around the model that makes the task repeatable). Managing the prompt without its harness is like filing the SOP but losing the work-cell layout it assumes. [src-101, src-208]
- 🏭 Manufacturing field example: a senior quality engineer spends three weeks refining a prompt that classifies casting surface defects exactly the way the plant's customers require. That prompt now contains her 15 years of judgment about what counts as "critical." If it stays in her personal chat history, the plant has just created — and is about to lose — a core process document.
- (Current scattered-prompt status in our affiliates → [fill from as-is])

### [Slide 3] What breaks without asset-ization — five structural failures

**👉 One-line summary:** Unmanaged prompts fail in the same five ways any unmanaged work document fails: they get lost, they diverge, nobody can audit them, nobody can reuse them, and they silently break when something underneath changes.

**Headline:** Each failure follows mechanically from "important procedure + no management system" — no statistics needed to see the logic.

- **① Knowledge loss** — the prompt lives in one person's chat history or local file; when they leave or move, the encoded judgment leaves with them. Like a veteran's know-how that never made it into the SOP. [src-304, src-301]
- **② Inconsistency** — Team A and Team B each wrote "the contract-review prompt"; both work, differently. Same task, different people, systematically different answers — and "a small wording change in a prompt can dramatically alter model behavior." [src-305]
- **③ No audit trail** — a prompt edit silently changes business outcomes. When an auditor asks "which instruction produced this AI decision, who approved it, when did it change?", there is no answer. Like missing lot traceability: the failure exists, the cause can't be reconstructed. [src-307]
- **④ No reuse** — every team and affiliate re-engineers the same prompt from scratch; quality varies; group-wide rollout has no foundation. Like every plant re-deriving its own fastening-torque spec instead of using the shared engineering library. [src-305, src-304]
- **⑤ Silent dependency breakage** — a prompt depends on a specific model version, data fields, terms, and tools. Providers update models without notice; the prompt "still runs" (API returns success) but the behavior shifts — **prompt drift** (output changes although the prompt text didn't). The failure is qualitative, so no alarm fires. [src-302, src-110]
- 🏭 Manufacturing field example: a document-classification workflow runs fine for four months. One Tuesday the model provider silently patches the model; the prompt that used to return clean JSON now wraps it in extra text; the downstream parser fails; debugging takes three days because nothing links "output changed" to "model changed." [src-302]

> ▸ **Backup:** [Backup 3-1] Evidence detail — prompt drift & prompt rot

#### [Backup 3-1] Evidence detail — prompt drift & prompt rot

| Phenomenon | Definition | Documented evidence | Source |
|---|---|---|---|
| Prompt drift | Output behavior changes over time with NO edit to the prompt | Stanford/UC Berkeley (Chen, Zaharia, Zou 2023): GPT-4 accuracy on prime-number identification fell 84% → 51% between Mar and Jun 2023, with no version change announced | src-302, src-110 |
| Prompt rot | Prompts implicitly overfit to one model version; a model swap breaks them | "Prompts are not contracts. They are hypotheses about how a specific model will interpret instructions." Symptoms: format violations, new verbosity, ignored few-shot examples | src-303 |
| Silent provider updates | Even dated model snapshots have changed behavior without notification | Early-2025 reports on a pinned, dated model version changing behavior; practitioners have additionally reported meaningful behavioral drift in deployed model services over multi-week windows (indirect citation via src-204) | src-110, src-204 |
| Chained-prompt drift | Updating one prompt changes the context every downstream prompt sees | "The generation prompt hasn't changed, but its outputs have." | src-110 |
| No-error failure mode | Drift throws no exception; API returns 200; logs look normal | "The failure is qualitative, not quantitative — a shift in tone, accuracy, or relevance that doesn't trigger any alert." | src-302 |

### [Slide 4] Key Objectives

**👉 One-line summary:** The goal is that AI work instructions are stored once, found by everyone, changed only with a trace, and kept working when the world underneath them moves.

**Headline:** Five objectives, each closing one of the five failures.

- **Consolidate** — one registry (central, versioned storage for prompt assets) as the single source of truth; no parallel personal copies. (closes ①) [src-305]
- **Standardize** — convert expert procedures into a common prompt structure (role / procedure / criteria / output / prohibitions) so the same task yields the same behavior. (closes ②) [src-205]
- **Govern** — every change carries who/what/why/approver; high-impact prompts deploy only after approval. (closes ③) [src-307, src-204]
- **Reuse** — package proven prompts + their execution context as harnesses that other teams and affiliates can adopt by parameter change, not rebuild. (closes ④) [src-208]
- **Protect** — record each prompt's dependencies (model version, data, terms, tools) and re-validate when any of them changes. (closes ⑤) [src-302, src-204]

---

## 3. When? — when it is needed

### [Slide 5] Timing & conditions

**👉 One-line summary:** Light version control is needed the moment any prompt affects real work; the full registry-and-governance system pays off when prompts cross teams, affiliates, or high-stakes decisions.

**Headline:** The trigger is not "how much AI we use" but "whether a prompt now stands between people and a business outcome."

- **Always needed (any production use):** the moment a prompt-driven task leaves the experiment stage and real users or decisions depend on it, three minimums apply — version control (otherwise rollback is impossible), separation of test vs. production versions, and a record of which prompt version produced which output. [src-301, src-305]
- **Conditional (full governance):** a formal registry with approval workflow and regression checks becomes necessary when ① multiple teams run the same workflow, ② tasks are delegated to agents (AI that acts in multiple steps on its own — its behavior IS its system prompt), ③ decisions are regulated or high-stakes, ④ multi-affiliate rollout begins, or ⑤ a model version upgrade is planned (the highest-risk single event for prompts). [src-301, src-302, src-307]
- Rule of thumb from practice: at roughly 3–5 production prompt workflows, scattered management becomes an operational liability. [src-301]
- 🏭 Manufacturing field example: one plant piloting an AI daily-report summarizer can live with a simple versioned folder. The moment HQ decides to roll the same summarizer out to eight plants — each with different report formats and terms — an unmanaged prompt becomes eight diverging prompts within a quarter, and the registry stops being optional.

---

## 4. What? — what it is (core)

### [Slide 6] Section agenda

**👉 One-line summary:** This section makes the topic itself fully understandable — what a prompt asset and a harness are, what they're made of, and how they live.

This section answers, in order:
- ① What exactly is a "prompt asset," and what is this topic NOT about? (definition & scope)
- ② What is inside a managed prompt asset? (anatomy + a fully worked example)
- ③ What is a "harness," and why is a prompt alone not enough? (the repeatable work unit)
- ④ Where do these assets live and how do they change? (registry & lifecycle)
- ⑤ What does a prompt depend on? (the dependency map)
- ⑥ Key glossary

### [Slide 7] Definition & scope — prompt asset, and what this topic is NOT

**👉 One-line summary:** A prompt asset is a work instruction for AI that has been promoted from a throwaway text into a managed document — with an ID, version, owner, and approval record.

**Headline:** D-3 is not about writing clever prompts; it is about managing the prompts that encode our work as controlled organizational documents.

- Plain definition: a **prompt asset** is a prompt elevated from a disposable string into a "managed, versioned, reusable artifact — with a formal schema, version history, ownership metadata, and a place in a central registry." [src-108]
- The contrast that defines the topic: **prompt engineering** (the craft of writing a good instruction, one task at a time) vs. **prompt asset management** (governing instructions as organization-wide assets). The first is a skill; the second is a data-management system. This guide covers the second. [src-112]
- What D-3 is NOT (scope boundary, kept deliberately strict):
  - HOW a tool works, its API and schema → **D-2** (D-3 only owns the instruction "when and in what order to use which tool")
  - The evaluation question sets and gold answers used to test prompts → **E-3** (D-3 only owns the *pointer* to them)
  - The ongoing collect-feedback-and-improve operation → **E-4** (D-3 owns creating the new version when improvement is decided)
- 🏭 Manufacturing field example: "write me a defect report" typed ad hoc into a chatbot is prompt *use*. The plant's standard defect-report-generation prompt — ID'd, versioned, owned by the quality team, approved by the QA manager — is a prompt *asset*. Same text, different status: one is a sticky note, the other is a controlled document.

> ▸ **Backup:** [Backup 7-1] Prompt engineering vs. asset management / [Backup 7-2] MECE boundary with D-2·E-3·E-4

#### [Backup 7-1] Prompt engineering vs. prompt asset management (detail)

| Dimension | Prompt engineering | Prompt asset management (D-3) |
|---|---|---|
| Goal | Craft better instructions for one task | Govern, version, and operationalize prompts as organizational assets |
| Scope | One-off, individual | Organization-wide, reusable, governed |
| Artifact | A string in code or chat | A versioned record in a registry with metadata |
| Skill | Linguistic, iterative | Systems engineering, data governance |
| Failure mode | Bad output for one task | Silent regressions at scale; compliance failures; knowledge loss on attrition |

(Source: src-112, src-108. The three nested layers: prompt engineering → context engineering → harness engineering — each layer's failure mode contains the previous one's. [src-112])

#### [Backup 7-2] MECE boundary with neighboring topics (detail)

| Asset element | D-3 owns | Neighboring topic owns |
|---|---|---|
| Instruction text, role definition, judgment criteria written into the prompt | ✅ | — |
| Tool-call order and conditions ("first search the taxonomy, then call vision API") | ✅ | D-2 owns the tool's own spec/schema |
| `tool_list` field (which tools this prompt may use) | ✅ pointer | D-2 owns the tool descriptions |
| Output schema the prompt must produce | ✅ | — |
| Eval dataset reference (`eval_set: defect-classifier-golden-v2`) | ✅ pointer | E-3 owns the dataset content |
| Regression run on each prompt change | ✅ the gate as part of prompt change control | E-3 owns test content; E-4 owns ongoing feedback operations |
| User-edit signals collected in operation | — | E-4 collects; D-3 consumes them as version-improvement input |

(Source: src-101, src-102 component mapping, src-112)

### [Slide 8] Anatomy of a prompt asset — what's inside

**👉 One-line summary:** A managed prompt is not one blob of text — it has named parts: who the AI acts as, what steps to follow, how to judge, what never to do, what shape the answer takes, plus the management label (version, owner, approval).

**Headline:** Eight content parts + one management envelope; once the parts are named, prompts stop being magic text and become editable, reviewable documents.

- The content parts, in plain words:
  - **Role definition** — the expertise frame ("you are a quality inspector for ...") that sets perspective and scope
  - **Instructions / procedure** — the ordered steps to perform
  - **Judgment criteria** — the explicit thresholds and priority tiers ("critical / major / minor, with cutoffs") — this is where human judgment becomes inspectable text
  - **Few-shot examples** (worked input→output samples placed inside the prompt to demonstrate the expected behavior) — calibration by example
  - **Output schema** (the formal answer format, e.g. a JSON structure = a machine-readable form with fixed fields) — what downstream systems parse
  - **Guardrails / prohibited actions** — "never pass an ambiguous case without escalation"
  - **Variables** (named placeholders like `{{batch_no}}` filled in at run time) — what makes one prompt reusable across products/teams
  - **Model & parameters** — which model version, with which settings, this prompt was validated on
- The management envelope: ID, version, owner, change reason, approver, status (draft/production/retired), dependency links. [src-108, src-103, src-104]
- 🏭 Worked example with real values (the level every key asset should reach):

| Field | Value |
|---|---|
| Prompt ID | `quality.inspection.defect-classifier` |
| Version | 2.1.0 (production) |
| Owner | Quality Innovation Team, Mgr. K. Park |
| Role | "QC inspector specializing in casting surface defects, per internal standard VS-002" |
| Judgment criteria | Scratch >5mm → flag; crack any size → flag; discoloration >10% of area → flag; severity Critical/Major/Minor with cutoffs |
| Prohibited | "Do not pass ambiguous cases — escalate to supervisor" |
| Output schema | JSON {defect_type, severity, location, measurement_mm, action_required} |
| Variables | {{product_id}}, {{batch_no}}, {{image_ref}} |
| Model (pinned) | claude-sonnet-4-6 (snapshot-pinned), temp 0.1 |
| Approved by / date | QA Part Leader J. Lee / 2026-05-20 |
| Depends on | defect taxonomy v3 (glossary), reference images VS-002, eval set defect-classifier-golden-v2 |

(adapted from src-205, src-210, src-204 examples)

> ▸ **Backup:** [Backup 8-1] Full field list of a managed prompt record / [Backup 8-2] Prompt-as-file: YAML schema example

#### [Backup 8-1] Full field list of a managed prompt record (detail)

**Identification & versioning:** `name/id` · `version/commit_hash` · `created_at` · `updated_at` · `created_by/owner` · `tags` · `change_reason`
**Content:** `system_prompt/template` · `role_definition` · `instructions` · `judgment_criteria/rubrics` · `few_shot_examples` · `output_schema` · `guardrails/prohibited_actions` · `input_variables`
**Model & execution:** `model` (pinned snapshot, not alias) · `temperature` · `max_tokens` · `tool_list` (→ D-2)
**Dependencies:** `depends_on_glossary_terms` · `depends_on_data_assets` · `depends_on_eval_dataset` (→ E-3) · `depends_on_prompts` (up/downstream in a chain)
**Governance:** `status` (draft/staging/production/deprecated) · `approved_by` · `approval_date` · `access_level` · `permitted_data_classes`
(Source: synthesis of src-103, src-104, src-108, src-111, src-107)

#### [Backup 8-2] Prompt-as-file: YAML schema example (detail)

```yaml
# defect_classifier.v2.1.yaml
name: DefectClassifier
description: "Classifies casting surface defects per VS-002."
version: 2.1.0
tags: ['quality', 'inspection']
input_variables: [product_id, batch_no, image_ref]
execution_settings:
  model: "claude-sonnet-4-6"        # pinned snapshot in production
  temperature: 0.1
  max_tokens: 1024
template: |
  You are a QC inspector specializing in casting surface defects (standard VS-002).
  Inspect {{image_ref}} for product {{product_id}}, batch {{batch_no}}.
  Judgment criteria: scratch >5mm → flag; crack any size → flag; ...
  If the case is ambiguous, do NOT pass it — escalate.
  Return JSON: {defect_type, severity, location, measurement_mm, action_required}
```
Three design principles behind the format: ① templates separate stable logic from runtime data; ② a formal schema makes the prompt a specification, not a guess; ③ files commit to version control — "if your prompt isn't in version control, it doesn't exist." (Source: src-108)

### [Slide 9] What a harness is — the prompt's complete work unit

**👉 One-line summary:** A harness is the prompt plus everything it needs to actually run the same way every time: which data comes in, which tools get called in what order, and what shape must come out.

**Headline:** "A harness is every piece of code, configuration, and execution logic that isn't the model itself" — manage the bundle, because the bundle is what actually does the work. [src-101]

- Why a prompt alone is an incomplete asset, in one cause-effect: the prompt says *how to judge*, but the result also depends on *what the AI saw* (input data, retrieved context) and *what it could do* (tools, order). If only the text is managed, a "perfect prompt" still fails when the surrounding pieces differ — and the failure gets misdiagnosed as a prompt problem. [src-208]
- What the harness bundles (each element glossed):
  - the **prompt(s)** by version (`defect-classifier@2.1.0`)
  - the **input data contract** — required fields and types it must receive
  - the **tool-call order** — the sequence/conditions for tool use (the tools themselves: D-2)
  - the **pinned model + parameters** — the exact model snapshot the bundle was validated on
  - the **output contract** — the schema downstream systems parse
  - the **eval set link** — which test set validates this bundle (content: E-3)
- 🏭 Manufacturing field example: a harness is a **standardized work cell**, not a machine setting. The cell sheet defines incoming material spec, the operation sequence, the decision points, and the outgoing inspection form. Anthropic's published harness for long-running agents encodes literally a **shift-changeover procedure**: each session reads the progress log and git history (the handover sheet), does one increment of work, and writes the handover for the next session. A human procedure, stored as an AI-executable asset. [src-109]

> ▸ **Backup:** [Backup 9-1] Harness component taxonomy / [Backup 9-2] Harness ≠ agent framework

#### [Backup 9-1] Harness component taxonomy (detail)

Six harness components (MongoDB taxonomy, Apr 2026) and their D-3 relevance [src-102]:

| Component | What it does | D-3 ownership |
|---|---|---|
| State & persistence | Keeps progress/checkpoints so work can resume | The *procedure* for state handling is D-3 (cf. Anthropic progress-file pattern, src-109) |
| Security & governance | Identity, permission scoping, auditability | Prompt-side guardrails & approval = D-3; access control systems themselves = C-2 |
| Orchestration & tool use | Drives the loop: reason → act (tool call) → observe → repeat | Tool-call order/conditions = D-3; tool specs = D-2 |
| Memory | Working context + persistent stores | What to include in context = D-3 instruction logic |
| Observability | Records decisions, calls, outcomes | Prompt-version tracking = D-3; execution tracing ops = E-4 |
| Evals | Out-of-band quality checks | Pointer & change-gate = D-3; datasets & criteria = E-3 |

#### [Backup 9-2] Harness ≠ agent framework (detail)

- An **agent framework** (LangChain, LangGraph, CrewAI...) is reusable software for building agents. A **harness** is the specific configuration for a particular task — it is the asset; the framework is the machine that runs it. [src-101, src-102]
- Evidence the harness itself carries performance: in a published benchmark comparison, the same model moved from ~Top 30 to Top 5 by changing only the harness — not the model, not the framework. [src-102]
- Industry shorthand for the layering: "Agent = Model + Harness." [src-101]

### [Slide 10] Where assets live — the registry and the asset lifecycle

**👉 One-line summary:** Prompt assets live in a registry — a controlled library where every save is a new version, every release is a label move, and going back is instant.

**Headline:** The lifecycle is borrowed from decades of document and software control: commit → review → promote → (if needed) roll back; published versions are never edited in place.

- **Prompt registry** (gloss: a centralized, versioned repository from which applications fetch prompts at run time, instead of having the text hard-coded inside them): one source of truth; updating the prompt does not require redeploying the application. [src-108, src-305]
- The lifecycle in four verbs, plain narrative:
  ① **Commit** — every save creates a new immutable version (old ones never deleted) →
  ② **Stage** — the draft is validated by a reviewer in a test environment →
  ③ **Promote** — the "production" label moves to the validated version (no code change) →
  ④ **Roll back** — if something breaks, the label moves back; recovery is a pointer change, not an emergency edit. [src-103, src-201]
- The one rule that makes audits possible: **immutability** — "once a prompt version is published to production, it must never be modified; any change — even a typo fix — creates a new version." Without it, "what prompt was running during that incident?" has no answer. [src-204]
- 🏭 Manufacturing field example: identical to controlled-document discipline in an ISO-certified plant — you never white-out a released SOP; you issue Rev. C, and the document board says which revision is in force on the line.

> ▸ **Backup:** [Backup 10-1] Registry minimum feature set & environment promotion detail

#### [Backup 10-1] Registry minimum feature set & environment promotion (detail)

Minimum viable registry [src-202, src-201, src-203]:
- **Storage**: immutable versioned artifacts (text + metadata)
- **Labels/tags**: draft / staging / production pointers, floating independently of version numbers
- **Fetch API**: retrieve by name + label (e.g., `pull("quality.inspection.defect-classifier:production")`)
- **Diff view**: side-by-side comparison of any two versions
- **Audit log**: who changed what, when, why, approved by whom

Environment promotion pattern (LangSmith reference implementation) [src-103]:
```
v3 commit → tag "staging" → validation → tag "production" moves to v3
rollback = move "production" tag back to v2 (instant, no deploy)
```
- Code references the tag, not the version hash → promoting/rolling back requires no code change.
- Webhook on commit → CI pipeline runs the eval suite → block promotion if quality drops. [src-103]
- Protected labels (enterprise feature): only authorized roles may move the `production` pointer. [src-201]

### [Slide 11] The dependency map — what a prompt silently relies on

**👉 One-line summary:** Every prompt quietly assumes a specific model version, certain data fields, standard terms, and tools — the dependency map writes those assumptions down so a change anywhere triggers a check, not an outage.

**Headline:** A prompt without a dependency record breaks invisibly; a prompt with one breaks loudly and early — which is the goal.

- What to record per prompt (each is a pointer, not a copy):
  - **Model**: the pinned snapshot it was validated on (an alias like "latest" is a silent time bomb)
  - **Data assets**: tables/fields/documents whose names and meanings the prompt text assumes
  - **Glossary terms** (→ A-3): the standard vocabulary the instructions use
  - **Tools** (→ D-2): what it calls, in what order
  - **Eval set** (→ E-3): which golden test set re-validates it
  - **Up/downstream prompts**: in chained workflows, whose output it consumes and who consumes its output [src-204, src-110]
- The payoff is **impact analysis**: "defect taxonomy term renamed → which prompts mention it?" / "model upgrade planned → which prompts must re-run their golden set first?" Without the map, these are company-wide guessing games. [src-204]
- 🏭 Manufacturing field example: a BOM (Bill of Materials = the structured parts list of a product) for judgment logic. When a supplier changes one material spec, the BOM tells you which products are affected; when one glossary term changes, the dependency map tells you which prompts are affected.

> ▸ **Backup:** [Backup 11-1] Dependency manifest example & impact rules by change type

#### [Backup 11-1] Dependency manifest example & impact rules (detail)

```yaml
prompt_id: quality.inspection.defect-classifier
version: 2.1.0
dependencies:
  model: claude-sonnet-4-6          # pinned snapshot, not floating alias
  data_assets: [defect_taxonomy_v3, reference_images_VS-002]
  glossary_terms: [defect_type, severity_grade]      # → A-3
  tools: []                                          # → D-2
  eval_set: defect-classifier-golden-v2              # → E-3
  downstream_consumers:
    - quality-dashboard-api   (parses field: severity)
    - capa-workflow-trigger   (reads field: action_required)
```
Impact rules when a dependency changes [src-204, src-202]:

| Change | Required response |
|---|---|
| Model upgrade | Re-run full eval suite; treat as potential MAJOR version event even if text unchanged |
| Glossary term renamed/redefined | Find all prompts referencing it; owner re-validates judgment criteria |
| Tool schema changed | Update prompts using it; output schema change = MAJOR bump |
| Few-shot examples replaced | MINOR if intent unchanged; MAJOR if judgment calibration shifts |
| Upstream prompt edited | Downstream prompts' inputs changed — re-run their evals despite "no edit" |

### [Slide 12] Key glossary

**👉 One-line summary:** Ten terms cover almost every conversation on this topic.

| Term | Easy one-line gloss (+ field context) |
|---|---|
| System prompt | The standing order given to AI before any user input — role, rules, output form. The AI-era work standard sheet. |
| Prompt template | Prompt text with named blanks (`{{batch_no}}`) filled at run time — one validated instruction reused across many runs. |
| Few-shot example | Input→output samples embedded in the prompt to show, not tell, the expected behavior — like attaching boundary-sample photos to an inspection standard. |
| Output schema | The fixed, machine-readable answer format (typically JSON) — what lets downstream systems consume AI output without a human re-reading it. |
| Guardrail | An explicit prohibition in the prompt ("never pass ambiguous cases") — the "do not" lines of an SOP. |
| Prompt registry | The controlled library of versioned prompt assets, fetched at run time — the plant's document control room for AI instructions. |
| Harness | Everything around the model that makes a task repeatable: prompt + input contract + tool order + output contract — the standardized work cell. |
| Dependency map | The written list of what a prompt relies on (model, data, terms, tools, eval set) — the BOM of judgment logic. |
| Prompt drift | Behavior change with no prompt edit (model/data moved underneath) — calibration drift for instructions. |
| Prompt regression | Worse behavior caused BY a prompt edit — caught by re-running the golden test set before release. |

(Source: src-101, src-108, src-110, src-302, glossary synthesis)

---

## 5. How? — how to asset-ize (core)

> Anchor: this is not "how to build AI." It is how to take the procedures and judgment criteria that already
> drive our work, and prepare them as structured, versioned, reusable data assets that any AI task can run on.

### [Slide 13] Section agenda

**👉 One-line summary:** This section is the to-do narrative: select → structure → register & govern → protect → package → operate, plus tools and pitfalls.

This section answers, in order:
- ① In what order do we asset-ize? (the 6-stage methodology)
- ② Which prompts do we manage first? (selection criteria)
- ③ How do we turn an expert's procedure and judgment into a structured prompt? (the conversion step)
- ④ How do we version and approve changes? (governance)
- ⑤ How do we keep assets from silently breaking? (dependencies & regression)
- ⑥ How do we package for reuse across teams and affiliates? (harness packaging)
- ⑦ Which tools support this, who operates it, and what goes wrong? (tooling / operations / pitfalls)

### [Slide 14] The asset-ization methodology — six stages

**👉 One-line summary:** Asset-ization runs like any data-preparation effort: find what exists, structure it, register it, connect it, package it, and keep it alive.

**Headline:** Inventory → Select → Structure → Register & Govern → Map & Protect → Package & Operate — each stage produces a concrete deliverable.

- ① **Inventory** — collect the prompts already in use (chat histories, shared docs, code) into one list. The asset already exists; it's just scattered. [src-207, src-407]
- ② **Select** — choose what deserves management (next slide); not everything needs the full treatment.
- ③ **Structure** — convert procedures and judgment criteria into the standard prompt anatomy (role / steps / criteria / prohibitions / output schema / variables) with SME (Subject Matter Expert = the person who actually masters the work) input. [src-205, src-210]
- ④ **Register & Govern** — put structured prompts in the registry; turn on versioning, approval, and environment labels. [src-204, src-103]
- ⑤ **Map & Protect** — record dependencies; wire the golden-set regression check (run the standard test set on every change and on schedule). [src-204]
- ⑥ **Package & Operate** — bundle proven prompts into harnesses; assign owners; run review/retirement cadence. [src-208]
- 🏭 Manufacturing field example: the same arc a plant followed when scattered Excel work standards became the controlled SOP system — collect, select the official ones, rewrite to standard format, register, control changes, retire the obsolete.

> ▸ **Backup:** [Backup 14-1] Per-stage deliverables & prerequisites

#### [Backup 14-1] Per-stage deliverables & prerequisites (detail)

| Stage | Key activity | Deliverable | Prerequisite |
|---|---|---|---|
| ① Inventory | Codebase/chat/doc sweep for prompts in actual use | Prompt inventory list (typical first-audit completeness: 40–60%) | Team cooperation; naming convention drafted |
| ② Select | Score by repetition, judgment impact, multi-team use, risk | Prioritized asset-ization backlog | Inventory complete |
| ③ Structure | SME extraction interview → standard anatomy | Structured prompt records (role/criteria/schema/variables) | SME availability; glossary (A-3) for standard terms |
| ④ Register & Govern | Registry setup; versioning, labels, approval flow | Registry live; CONVENTIONS.md; approval workflow | Tool selection; naming finalized BEFORE evals (see pitfalls) |
| ⑤ Map & Protect | Dependency manifests; golden-set regression wiring | Dependency map; CI gate + scheduled regression run | Eval sets exist (E-3); model versions pinned |
| ⑥ Package & Operate | Harness bundling; ownership; review cadence | Harness registry entries; lifecycle policy (LIFECYCLE.md) | Stages ①–⑤ stable for the pilot scope |

(Source: src-407, src-204, src-205, src-208)

### [Slide 15] Stage ② — selecting what to manage

**👉 One-line summary:** Manage first the prompts that are used repeatedly, carry judgment that affects business outcomes, or are about to be shared — one-off personal prompts can stay personal.

**Headline:** Four selection criteria; if two or more apply, it belongs in the registry.

- **Repetition** — the same prompt (or its copies) runs weekly or daily; every unmanaged repeat compounds divergence.
- **Judgment impact** — the prompt's criteria decide pass/fail, classification, escalation, or anything a customer/auditor could later question.
- **Shared use** — more than one person/team runs it, or an affiliate rollout is planned; sharing without a single source guarantees divergence (the copy-paste failure). [src-204]
- **Risk class** — regulated, safety-related, or external-facing outputs get governance regardless of frequency. Tiered governance: heavyweight review only for high-impact prompts — applying full process to everything creates bottlenecks; applying none is negligence. [src-204, src-207]
- 🏭 Manufacturing field example: the weekly equipment-anomaly triage prompt used by all maintenance crews across two plants = clear asset (repetition + judgment + shared). An engineer's one-off "summarize this paper" prompt = not an asset. The defect-verdict prompt a customer audit may probe = asset even if used monthly.

### [Slide 16] Stage ③ — turning procedures & judgment into structured prompts

**👉 One-line summary:** The heart of asset-ization: sit with the expert, pull the unwritten criteria out of their head, and write them into the prompt's named parts — the act of writing is itself where hidden judgment becomes explicit.

**Headline:** "The act of writing surfaces assumptions that were never articulated — distinctions intuitive for the senior must become precise for the AI." [src-205]

- Step 1 — **extract**: a structured interview with the SME, organized around questions like "what should never be left to individual judgment?", "what separates a good output from a problematic one?", "what triggers immediate escalation?" Answers map directly to prompt parts: non-negotiables → guardrails; recurring corrections → explicit check items; instinct → priority-ranked criteria. [src-205]
- Step 2 — **structure** into the four-part frame (gloss: PTCF = Persona·Task·Context·Format): who the AI acts as / exactly what to do / what real-world grounding it gets / what shape the answer takes — plus judgment criteria in priority tiers (must / should / nice-to-have) and prohibited actions. [src-210, src-205]
- Step 3 — **calibrate with examples**: attach worked input→output samples (few-shots), especially for borderline cases — the prompt equivalent of boundary-sample photos in an inspection standard.
- A side benefit seen in practice: extraction surfaces disagreements experts never knew they had — two senior engineers were found to apply quietly different thresholds for "critical"; writing one shared instruction forced the standard to be settled. The company gains a clarified standard even before the AI runs. [src-205]
- 🏭 Worked example — before/after:
  - Before (what the SOP said): "Inspect for defects."
  - After (prompt-ready): role = "QC inspector per VS-002"; criteria = "scratch >5mm flag / crack any size flag / discoloration >10% area flag; severity cutoffs Critical–Major–Minor"; prohibited = "never pass ambiguous cases without escalation"; output = fixed JSON; variables = product, batch, image. (= Slide 8's record) [src-210, src-205]

> ▸ **Backup:** [Backup 16-1] SME extraction toolkit / [Backup 16-2] Structuring conventions for maintainability

#### [Backup 16-1] SME extraction toolkit (detail)

Interview question set [src-205]:
- What decisions should never be left to individual judgment?
- Which criteria do experienced staff apply instinctively but never write down?
- What triggers an immediate escalation or rejection?
- What separates a satisfactory output from a problematic one?
- Which mistakes recur in less-experienced staff's work?

Supplementary elicitation techniques [src-210]:
- **Think-aloud protocol** — expert narrates reasoning while performing the task
- **Storytelling** — memorable edge cases and how they were resolved
- **Decision tables** — enumerate condition combinations → actions
- **Failure-mode review** — past failures and the correct response
- **Contrast analysis** — explain a good output vs. a borderline output side by side; the explanation IS the criteria

#### [Backup 16-2] Structuring conventions for maintainability (detail)

- **Four-layer separation** (role / instruction logic / data contract / output schema) — each layer independently modifiable: persona change doesn't touch logic; schema change doesn't require re-eliciting knowledge. [src-205, src-203]
- **Template variables** for all runtime content → reuse across product lines/locales, and testability with known values. [src-201]
- **Composability** — shared blocks (tone guide, standard disclaimer) maintained once, referenced by many prompts (e.g., Langfuse `@@@langfusePrompt:name=...@@@` syntax). [src-202]
- **Naming convention** — `domain.process.function` (e.g., `quality.inspection.defect-classifier`). [src-203]
- **Style guide** — standard section order, persona vocabulary, complexity threshold for splitting into sub-prompts, mandatory metadata header. [src-203, src-205]

### [Slide 17] Stage ④ — versioning and change governance

**👉 One-line summary:** Treat a prompt release like a document revision: published versions are never edited, every change states its reason and approver, and big/small changes are labeled differently so consumers know what to expect.

**Headline:** Immutability + meaningful version numbers + an approval path proportionate to impact.

- **Immutability** (restated as the working rule): a production version is frozen; even a typo fix is a new version. Rollback = moving the pointer. [src-204]
- **Version semantics** in plain words (gloss: SemVer = MAJOR.MINOR.PATCH numbering):
  - changed the output format, role, or logic → **MAJOR** (downstream consumers must be told)
  - added a criterion or example, backward compatible → **MINOR**
  - wording/typo with no behavior change → **PATCH** — and if a "patch" measurably changes behavior in the test run, it wasn't a patch. [src-204]
- **What versions together**: the text alone is not the version — model snapshot, parameters, tool definitions, and the change rationale are part of the release. A model swap with unchanged text is still a (potentially MAJOR) change. [src-204]
- **Approval path**: author drafts freely → automated structure check → SME validates behavior intent → eval gate (golden-set run; block on regression) → for high-impact/regulated prompts, compliance sign-off → promote. Non-engineers can author; promotion always passes the gates. [src-204, src-209]
- 🏭 Manufacturing field example: identical in spirit to ECN discipline (Engineering Change Notice = the formal record that a drawing changed, why, and who approved): no line runs on a hand-edited drawing, and no production task should run on a hand-edited prompt.

> ▸ **Backup:** [Backup 17-1] SemVer trigger table & approval workflow detail

#### [Backup 17-1] SemVer trigger table & approval workflow (detail)

| Bump | Prompt trigger examples | Why it matters |
|---|---|---|
| MAJOR (X.0.0) | Output format change, role rewrite, model switch, logic restructuring | Breaks downstream parsers/processes; consumers need advance notice |
| MINOR (x.Y.0) | New judgment criterion, added few-shot, new optional output field | New capability, backward compatible |
| PATCH (x.y.Z) | Typo, wording clarity, whitespace | Must NOT materially alter behavior (verified by eval run) |

(Source: src-204, src-203)

Example seven-step approval workflow for high-impact prompts (synthesized from the roles and gates described in src-204, src-209):
1. Author creates new version in draft
2. Automated structural validation (schema check, variable binding)
3. SME/domain reviewer approves behavioral intent
4. AI platform team reviews technical risk
5. Automated eval gate (golden set; block if drop exceeds tolerance, e.g. >3%)
6. Compliance sign-off (regulated contexts)
7. Promotion via label move — no code deploy

Resolution of the speed-vs-control tension: domain owners iterate through a GUI without engineering; the engineering-owned eval gate is the non-negotiable door to production. [src-204]

### [Slide 18] Stage ⑤ — keeping assets alive: dependencies & regression

**👉 One-line summary:** Two habits keep prompt assets from silently rotting: write down what each prompt depends on, and re-run its standard test set both on every change and on a schedule.

**Headline:** You cannot stop the model or the data from changing under you — you can make sure the change shows up in a test report instead of a customer complaint.

- Habit 1 — **dependency manifests** (Slide 11): every asset lists its pinned model, data, terms, tools, eval set, and consumers; any change to a listed item triggers re-validation of the prompts that point to it. [src-204]
- Habit 2 — **golden-set regression** (gloss: golden set = the standard collection of representative inputs + expected outputs for this task; content owned by E-3): run it ① before any version is promoted (catches *regression* — damage from our own edit) and ② on a schedule (catches *drift* — damage from silent changes underneath). Add every production failure case to the set so it never recurs untested. [src-204]
- Watch the human signal too: **user-edit rate** — when users increasingly correct a prompt's output, the prompt's criteria no longer match reality; route the signal (collected by E-4) to the prompt owner as improvement input. [src-203]
- 🏭 Manufacturing field example: this is SPC thinking (Statistical Process Control = sampling output daily to catch drift before defects ship) applied to judgment logic: the daily regression run is the control chart; an alert at "score dropped beyond tolerance" is the out-of-control signal that triggers investigation — before the defect batch ships. [src-401]

### [Slide 19] Stage ⑥ — harness packaging: making it reusable

**👉 One-line summary:** Once a prompt and its surroundings are proven, freeze them together as a harness — a named, versioned bundle another team or affiliate can adopt by changing parameters instead of rebuilding.

**Headline:** The harness is the unit of reuse: prompt + input contract + tool order + pinned model + output contract + eval link, registered as one asset with one version.

- Why bundle: the harness version captures what the prompt version cannot — that *this* prompt, with *this* model, *these* tools in *this* order, against *this* output contract, was validated as a working whole. "The 'prompt failed' diagnosis almost always turns out to be a harness failure." [src-208]
- **Parameterize for reuse**: the base harness holds the validated logic; affiliates register variants by overriding parameters — taxonomy reference, output language, severity threshold per customer contract, model tier — under a namespaced ID (`...defect-classifier.affiliate-A-v1`). Adoption becomes configuration, not re-engineering. [src-208 pattern]
- **Register harnesses like prompts**: searchable by domain/process/function, documented (purpose, contracts, owner, dependencies, eval link), lifecycle-managed (active → deprecated → retired).
- 🏭 Worked example with real values:

| Harness field | Value |
|---|---|
| Harness ID / version | `quality.inspection.defect-classifier-harness` / 1.3.0 |
| Prompt | `quality.inspection.defect-classifier@2.1.0` |
| Input contract | {product_id: str, batch_no: str, image_ref: url} |
| Tool-call order | ① search defect taxonomy → ② call vision analysis → ③ look up reference images |
| Model (pinned) | claude-sonnet-4-6 snapshot, temp 0.1 |
| Output contract | JSON schema v2.1 (severity field consumed by quality dashboard) |
| Eval link | defect-classifier-golden-v2 (E-3) |
| Owner / status | Quality Innovation Team / production; affiliate variants: A-plant (KR), B-plant (US, EN output, customer-X severity table) |

(Structure: src-208, src-206 worker-agent pattern)

### [Slide 20] Tool guidepost — what supports this

**👉 One-line summary:** Every major AI platform now ships prompt-management features, so the choice follows your existing stack — the discipline matters more than the product.

**Headline:** If you're on a cloud AI platform, use its native prompt management; if you want neutrality, use an open-source registry; if you're git-centric, files + CI can carry you far. Prompt versioning is now baseline enterprise infrastructure, not niche tooling. [src-111, src-105]

- The capability checklist matters more than the brand: immutable versions / labels & promotion / diff view / audit log / eval-gate hook / dependency metadata. (Buy any tool lacking these and you'll rebuild them by hand.) [src-202, src-201]
- 🏭 Manufacturing field example: same logic as choosing a document-management system for SOPs — the revision-control discipline transfers; the vendor is secondary.

> ▸ **Backup:** [Backup 20-1] Tool map — prompt management & harness solutions

#### [Backup 20-1] Tool map (detail)

| Solution | Vendor | Category | Key features | Self-host | Notes |
|---|---|---|---|---|---|
| Langfuse | Langfuse GmbH | Open-source LLMOps | Auto versioning + labels, RBAC, composability, diff, protected labels (EE) | Yes | MIT license; frequently recommended neutral default [src-201, src-202] |
| MLflow Prompt Registry | Databricks/Linux Foundation | MLOps platform | Versioning, aliases, eval integration | Yes | Natural fit if already on MLflow/Databricks [src-202, src-305] |
| Amazon Bedrock Prompt Management | AWS | Cloud-native | Visual builder, variant comparison, versioning, Flows integration | No | AWS-stack fit [src-111, src-206] |
| Azure AI Foundry (Prompt Flow) | Microsoft | Cloud-native | Flow-level versioning, variant testing, eval pipelines | No | Versions whole flows, not individual prompts [src-202] |
| LangSmith | LangChain | SaaS | Commit/tag versioning, env promotion, webhooks, playground | No | Deep LangChain integration [src-103] |
| PromptLayer | PromptLayer | SaaS registry | Low-friction registry, release labels, logging | Enterprise | Easy start [src-202] |
| Braintrust | Braintrust Data | SaaS | Content-addressed versions, A/B, strong eval link, CI action | No | Eval-centric [src-105] |
| Git-based (files + CI) | — | DIY | Full audit via PRs, diff, CI eval gates | Own infra | Lowest lock-in; needs custom fetch/runtime layer [src-204] |
| Datadog Prompt Tracking | Datadog | Observability | Version-linked traces, regression diff views, incident isolation | No | Evidence these KPIs are standard product features [src-409] |

(Humanloop, an influential governance-workflow pioneer, was acquired by Anthropic in Aug 2025 and sunset as a product — its review/approval patterns are now common across the tools above. [src-105, src-209])

### [Slide 21] Operations & governance — who keeps it alive

**👉 One-line summary:** Prompt assets need the same operating roles as any controlled document system: a named owner per asset, a reviewer for intent, a platform team for the registry, and a cadence for review and retirement.

**Headline:** "Prompts sit at the intersection of product intent, legal interpretation, and technical execution — no single existing role owns them naturally," so name the roles explicitly. [src-204]

- **Prompt/harness owner** (domain side, exactly one named person): accountable for criteria correctness, approves intent changes, receives the alerts. Shared ownership = no ownership. [src-401, src-204]
- **AI/data platform org**: runs the registry, sets conventions and style guide, owns the eval-gate infrastructure, manages cross-team reuse.
- **SME reviewer / compliance reviewer**: validates behavior change; signs off in regulated contexts.
- **Cadence**: every change → automated gates; monthly → owner reviews usage/edit-rate/escalations; quarterly → portfolio review (unused, redundant, underperforming prompts); event-driven → any model-provider update triggers re-evaluation across production prompts. [src-204]
- **Retirement**: lifecycle states active → deprecated (no new development, consumers notified) → retired (archived, replacement documented). Skipping retirement is how prompt sprawl returns. [src-207]
- 🏭 Manufacturing field example: mirror the plant's document-control committee: every SOP has an owner, an annual review date, and a formal obsolescence process — nobody asks "is this revision still valid?" mid-shift.

### [Slide 22] Pitfalls — the failure modes to design against

**👉 One-line summary:** Most failures are organizational, not technical: prompts scattered everywhere, copies that diverge, text hard-coded in apps, no owner, untested changes, and registries that exist but get bypassed.

**Headline:** Every pitfall below was documented in real deployments — design the system so the easy path is the governed path.

- **Prompt sprawl** — prompts in chats, wikis, mails; hundreds of conflicting variants before leadership notices. Countermeasure: inventory first, registry as the only fetch path. [src-207]
- **Copy-paste divergence** — Team B copies Team A's prompt; 12 variants in months; improvements never propagate. Countermeasure: shared base + parameterized variants (Slide 19). [src-204]
- **Hard-coded prompts** — text inside application code: every fix needs a deploy, domain experts can't participate, history is buried. Countermeasure: registry fetch at run time. "Treat every hardcoded prompt as a high-severity bug." [src-108, src-204]
- **No owner** — postmortems end at "someone changed it in a DM." Countermeasure: owner field mandatory; alerts routed to the owner, not a shared channel (shared-channel alerts go unread). [src-204, src-407]
- **Untested changes** — three words added "for conversational flow" → structured-output errors spiked within hours → revenue workflow halted. Countermeasure: eval gate on every promotion, no exceptions. [src-404, src-204]
- **Governance theater** — a registry nobody uses because the workflow is slower than bypassing it. Countermeasure: tiered governance (heavy review only for high-impact prompts) and tooling that makes the governed path the fastest path. [src-204, src-207]
- 🏭 Manufacturing field example: every one of these has a plant-floor twin — uncontrolled local copies of work standards, drawings edited by hand at the line, "everyone's responsibility" equipment that no one maintains. The cures transferred too.

---

## 6. KPI? — management metrics

### [Slide 23] Management KPIs (4)

**👉 One-line summary:** Four numbers tell you whether prompts are becoming assets: how much is under management, how much is test-protected, whether changes pass gates before going live, and whether new tasks reuse the library.

**Headline:** Coverage first (you can't govern what isn't registered), protection second, discipline third, reuse as the value proof.

| KPI | Easy meaning | Measurement purpose | Target dir. | Expected effect |
|---|---|---|---|---|
| Registry coverage | % of production prompts under management (owner, version, status filled) | Make the invisible inventory visible; denominator for everything else | ↑ (>95%) | No more unknown prompts in production; audit-ready baseline [src-401] |
| Eval coverage | % of managed prompts wired to a passing golden test set | Know which assets are protected against drift/regression | ↑ (>80%) | Model migrations become routine checks instead of panic weeks [src-401] |
| Gated-change rate | % of production prompt changes that passed the eval gate before deploy | Enforce change discipline; catch regressions pre-release | ↑ (→100%) | Untracked-change incidents approach zero [src-404] |
| Reuse rate / time-to-onboard | % of new AI tasks built from existing harnesses; days to stand up a new task | Prove the library pays back; measure scale-out speed | reuse ↑ / days ↓ | New tasks start from validated bundles, not from zero [src-406, src-403] |

- Why these four: coverage without protection is a phone book; protection without change discipline leaks at deploy time; all three without reuse means the asset never returns value. (Watch user-edit rate as a fifth, operational signal via E-4.)
- 🏭 Manufacturing field example: read them as SOP compliance rate, % of processes under SPC, ECN-discipline adherence, and standard-part reuse rate — the plant already manages by these instincts. [src-401, src-404]

> ▸ **Backup:** [Backup 23-1] KPI formulas & thresholds

#### [Backup 23-1] KPI formulas & thresholds (detail)

```
Registry coverage   = prompts in registry with complete metadata / all production prompts × 100
                      (count the denominator by codebase sweep, not registry self-report)
Eval coverage       = prompts with ≥1 passing eval suite wired to CI + schedule / all production prompts × 100
                      (binary per prompt; partial suites count as 0 to prevent gaming)
Gated-change rate   = changes promoted through eval gate / all production prompt changes × 100
Daily regression    = prompts whose scheduled eval dropped beyond tolerance / prompts evaluated × 100
                      (tolerance ≈ 2 points/100; sustained >2% = structural problem; spike >5% = investigate)
Change lead time    = deployment timestamp − authoring timestamp (registry+CI: minutes–hours vs. days)
```
Reference thresholds: first-audit registry coverage typically 40–60%, reaching 90%+ within weeks once the metric is owned; eval coverage <50% = "library is a wish," >80% = measurable system. [src-401, src-407]

---

## 7. Roadmap — scale-up roadmap

### [Slide 24] Short / mid / long term

**👉 One-line summary:** Build in the proven order — first make prompts findable and owned, then versioned and gated, then packaged and reused — because evals written before naming stabilizes get thrown away.

**Headline:** Three phases that match how external maturity models stage this capability; sequencing is the success factor, not speed.

- **Short term — Inventory & registry (foundation):** sweep code/chats/docs for prompts in use; fix naming convention and one owner per prompt; migrate to a single registry source. Deliverable: registry coverage baseline → 90%+. (Do NOT write evals yet — renames during migration would invalidate them; the #1 rollout failure.) [src-407]
- **Mid term — Versioning, approval & regression (protection):** immutable versions with labels; approval workflow proportionate to impact; golden-set eval gate on promotion + scheduled regression runs; dependency manifests on key assets. Deliverable: gated-change rate → 100%, eval coverage >80% on high-stakes prompts. [src-204, src-401]
- **Long term — Harness packaging & group-wide reuse (value):** bundle proven prompt+context+tool-order sets as harnesses; parameterized affiliate variants; searchable harness registry; integrate with E-3 (eval sets) and E-4 (operation feedback) into one improvement loop. Deliverable: reuse rate and time-to-onboard become the managed numbers. [src-208, src-406]
- External benchmark for the staging: Microsoft's GenAIOps maturity model lists "strengthen your asset management strategies through advanced version control and rollback capabilities" as the key advancement action from Level 3 (Managed) to Level 4 (Optimized) — meaning prompt asset-ization is the threshold capability for reaching L4, and our mid-term maps to the L3→L4 inflection; IBM's 5-phase model puts systematic prompt governance at Phase 3 — both locate our mid-term as the recognized inflection point from experimentation to operations. [src-410, src-408]
- 🏭 Manufacturing field example: same rollout arc as the SOP digitization program — document inventory first, revision control second, cross-plant standard library last; plants that skipped step one re-did the whole program.

> ▸ **Backup:** [Backup 24-1] 30/60/90-day pilot plan & maturity model detail

#### [Backup 24-1] 30/60/90-day pilot plan & maturity models (detail)

90-day pilot pattern (practitioner-validated) [src-407]:
- **Days 1–30 (Catalog + versioning):** codebase sweep → inventory; naming + version convention (CONVENTIONS.md); migrate to single source; one named owner per prompt; team demo. Metric: catalog completeness baseline.
- **Days 31–60 (Eval loop):** first golden suite on the highest-stakes prompt (happy path + edge + known failures); CI gate blocking regressions; nightly regression run with owner-routed alerts; model-fit test on top 5 prompts. Metrics: eval coverage, daily regression rate.
- **Days 61–90 (Governance + lifecycle):** new-prompt template (eval suite + owner + lifecycle stage mandatory); LIFECYCLE.md (beta/production/deprecated/archived + deletion dates); 1-hour training; monthly governance forum; one independent week before declaring done. Metrics: deprecation latency, stage accuracy.
- Failure modes to avoid: evals before naming finalized (#1); alerts to shared channels instead of owners (#2). [src-407, src-401]

Synthesized maturity staging [src-402, src-408, src-405, src-410]:

| Stage | Label | Prompt/harness state |
|---|---|---|
| 0 | Ad-hoc | Hard-coded/scattered; tribal knowledge |
| 1 | Inventoried | Findable, owned, named (catalog coverage >80%) |
| 2 | Versioned registry | Reproducible, debuggable; basic eval on high-stakes prompts |
| 3 | Governed (CI/regression) | Gated changes, scheduled regression, lifecycle policy — IBM Phase 3 inflection; Microsoft frames advanced version control/rollback as the L3→L4 advancement action |
| 4 | Harness reuse at scale | New tasks composed from existing assets; reuse and onboarding time managed as KPIs |

---

## Appendix

### [Appendix A] Prompt asset record — full field checklist

Use as the registration template (fields per Backup 8-1; sources src-103, src-104, src-107, src-108, src-111):

| Group | Field | Required |
|---|---|---|
| Identity | name/id (`domain.process.function`) | ● |
| Identity | version (SemVer) / change_reason | ● |
| Identity | owner (exactly one named person) | ● |
| Content | role_definition | ● |
| Content | instructions (ordered steps) | ● |
| Content | judgment_criteria (priority-tiered, with thresholds) | ● |
| Content | guardrails / prohibited_actions | ● |
| Content | output_schema | ● |
| Content | few_shot_examples (incl. borderline cases) | ○ |
| Content | input_variables (names + types) | ● |
| Execution | model (pinned snapshot) + parameters | ● |
| Execution | tool_list (pointers → D-2) | ○ |
| Dependency | depends_on: data_assets / glossary_terms (→A-3) / eval_set (→E-3) / prompts | ● for production |
| Governance | status / approved_by / approval_date / access_level | ● for production |

### [Appendix B] SME extraction interview sheet

Core questions (src-205) + techniques (src-210) from Backup 16-1, formatted for field use: run 60–90 min per task with the task's acknowledged expert; capture answers directly into the Appendix A template; close with contrast analysis on two real outputs (one good, one borderline); have a second expert review the drafted criteria — disagreements found here are standards waiting to be settled, not interview failures.

### [Appendix C] External maturity model reference

- **Microsoft GenAIOps maturity model** (official assessment, score 0–28): L1 Initial (0–9) → L2 Defined (10–14) → L3 Managed (15–19) → L4 Optimized (20–28). The L3→L4 advancement actions explicitly include "strengthen your asset management strategies through advanced version control and rollback capabilities" — placing advanced prompt asset-ization as the L4 threshold, not a characteristic of L3. Organizations at L3 are managing advanced LLM workflows with proactive monitoring; reaching L4 (operational excellence) requires adding version control and rollback for prompt assets. [src-410, src-402]
- **IBM GenAI adoption 5-phase model**: governance column reaches "common metrics to govern the AI lifecycle" at Phase 3 — where prompt versioning/registry shifts from optional to operational requirement; Phase 4 adds automated validation; Phase 5 adds prompt security monitoring. [src-408]

---

## References

- [The Anatomy of an Agent Harness (LangChain)](https://www.langchain.com/blog/the-anatomy-of-an-agent-harness) — src-101
- [The Agent Harness: Why the LLM Is the Smallest Part (MongoDB)](https://www.mongodb.com/company/blog/technical/agent-harness-why-llm-is-smallest-part-of-your-agent-system) — src-102
- [Manage prompts — LangSmith Docs](https://docs.langchain.com/langsmith/manage-prompts) — src-103
- [Prompt Version Control (Kore.ai)](https://www.kore.ai/blog/why-prompt-version-control-matters-in-agent-development) — src-104
- [The 5 best prompt versioning tools in 2025 (Braintrust)](https://www.braintrust.dev/articles/best-prompt-versioning-tools-2025) — src-105
- [Open Source Prompt Management (Langfuse Docs)](https://langfuse.com/docs/prompt-management/overview) — src-106
- [Enterprise AI Prompt Governance (PromptAnthology)](https://www.promptanthology.com/blog/enterprise-ai-prompt-governance) — src-107
- [The Prompt Lifecycle (NeoSage)](https://blog.neosage.io/p/the-prompt-lifecycle-every-ai-engineer) — src-108
- [Effective harnesses for long-running agents (Anthropic)](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) — src-109
- [Prompt Drift: What It Is and How to Detect It (Agenta)](https://agenta.ai/blog/prompt-drift) — src-110 / src-302
- [Construct and store reusable prompts (Amazon Bedrock Docs)](https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-management.html) — src-111
- [Prompt vs Context vs Harness Engineering (Atlan)](https://atlan.com/know/harness-engineering-vs-prompt-engineering/) — src-112
- [Langfuse Prompt Version Control Docs](https://langfuse.com/docs/prompt-management/features/prompt-version-control) — src-201
- [Prompt Management Systems Compared (Nearform)](https://nearform.com/digital-community/prompt-management-systems-compared/) — src-202
- [Managing Prompts as Enterprise Assets (S. Shmueli)](https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327) — src-203
- [Prompt Versioning and Change Management in Production (Tian Pan)](https://tianpan.co/blog/2026-03-13-prompt-versioning-change-management-production) — src-204
- [Encoding Team Standards (R. Garg, martinfowler.com)](https://martinfowler.com/articles/reduce-friction-ai/encoding-team-standards.html) — src-205
- [Amazon Bedrock Prompt Management (AWS)](https://aws.amazon.com/bedrock/prompt-management/) — src-206
- [From Prompt Sprawl to Governance (CFA Tech)](https://cfatech.ng/from-prompt-sprawl-to-governance-bringing-order-to-chaos) — src-207
- [From Prompts to Harnesses (J. Kim)](https://bits-bytes-nn.github.io/insights/agentic-ai/2026/04/05/evolution-of-ai-agentic-patterns-en.html) — src-208
- [Humanloop Prompt Management (historical)](https://humanloop.com/platform/prompt-management) — src-209
- [SOP-to-Prompt Conversion (Klariti)](https://klariti.com/2025/04/14/getting-started-guide-to-prompt-engineering-for-sops/) — src-210
- [The Definitive Guide to Prompt Management Systems (Agenta)](https://agenta.ai/blog/the-definitive-guide-to-prompt-management-systems) — src-301
- [Prompt Rot (V. Yadav)](https://medium.com/@yadavvaibhav0104/prompt-rot-why-your-perfect-prompts-stop-working-and-how-to-fix-them-3d45dd79cc60) — src-303
- [Why Large Enterprises Need a Prompt Registry (M. Rodek)](https://medium.com/@martin_rodek/why-large-enterprises-need-a-prompt-registry-for-ai-governance-04d744039bb4) — src-304
- [MLflow Prompt Registry](https://mlflow.org/prompt-registry) — src-305 / src-403
- [Prompt Engineering & Management in Production (ZenML)](https://www.zenml.io/blog/prompt-engineering-management-in-production-practical-lessons-from-the-llmops-database) — src-306 / src-406
- [Audit Trails for Accountability in LLMs (arXiv, Brown Univ.)](https://arxiv.org/html/2601.20727v1) — src-307
- [The Semver Lie (Tian Pan)](https://tianpan.co/blog/2026-04-29-semver-lie-llm-minor-update-breaks-production) — src-310
- [Prompt Library Metrics: Coverage & Regression Framework (Digital Applied)](https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026) — src-401
- [LLMOps Maturity Model (Microsoft Azure)](https://azure.microsoft.com/en-us/blog/achieve-generative-ai-operational-excellence-with-the-llmops-maturity-model/) — src-402
- [How Prompt Updates Drive Most Incidents (Deepchecks)](https://deepchecks.com/llm-production-challenges-prompt-update-incidents/) — src-404
- [LLMOps Maturity Models (ZenML)](https://www.zenml.io/blog/everything-you-ever-wanted-to-know-about-llmops-maturity-models) — src-405
- [Prompt Library Team Rollout: 30/60/90-Day Plan (Digital Applied)](https://www.digitalapplied.com/blog/prompt-library-team-rollout-30-60-90-day-plan-2026) — src-407
- [IBM Maturity Model for GenAI Adoption](https://www.ibm.com/think/architectures/patterns/genai-adoption-maturity-model) — src-408
- [Prompt Tracking — Datadog Docs](https://docs.datadoghq.com/llm_observability/monitoring/prompt_tracking/) — src-409
- [Advance your maturity level for GenAIOps (Microsoft Learn)](https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity?view=azureml-api-2) — src-410


