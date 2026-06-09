# D-3 Prompt/Harness Asset Management — AI-Ready Data Guide Storyline (English working draft)

> Source for slides. **Main slides = the correct storyline built from what's needed to understand.** `[Slide N]`
> **Backup slides = detail you drill into from a main when needed.** `[Backup N-x]` (directly under that main)
> Every main slide must include ① one-line summary ② inline term gloss ③ manufacturing field example.
> Push hard tables/specs/schemas/tool comparisons to backup, not the main.
> Topic = "preparing the prompt/harness as a managed DATA ASSET for AI to use reliably" — NOT "building AI" or "agent architecture."
>
> **Scope note:** Versioning, lineage/dependency tracking, metadata management, data quality gates, and security are generic data-asset disciplines covered in other topics (A-2 Metadata, C-3 Lineage, C-2 Quality, F-4 Security). This guide compresses references to those disciplines to brief pointers and focuses on what is **unique** to prompts and harnesses.

---

## 1. Executive Summary

### [Slide 1] One-slide summary: What is D-3 and why does it matter?

**👉 One-line summary:** Prompts and harnesses encode expert work procedures and judgment criteria — they must be captured, structured, and governed as organizational assets so that knowledge is not lost and AI results are reusable across teams.

**Headline:** Expert judgment written into a prompt is not yet an asset. An asset has an anatomy, an owner, a harness, and a governed home in the registry — and that takes deliberate work.

- **Definition:** D-3 Prompt/Harness Asset Management is the practice of converting human expert work procedures and judgment criteria into structured, reusable prompt and harness artifacts — and then governing those artifacts as first-class organizational data assets. The focus is on **what is unique to prompts and harnesses**: capturing tacit judgment as structured prompt anatomy, building harnesses that make a full work-unit reusable, and maintaining the fidelity of that judgment logic over time.
- **Why now:** Most organizations have written prompts down somewhere — but "written down" is Stage 2; a managed asset is Stage 3. The gap between Stage 2 and Stage 3 is the value proposition of D-3: a prompt without explicit judgment criteria, a defined output contract, and a reusable harness is brittle — it will drift from the real procedure, be re-written ad hoc by every team, and the expert's knowledge will disappear when they leave.
- **Key preparation work:** ① Elicit tacit work procedure + judgment criteria from SME → structured prompt anatomy ② Define output contract + guardrails ③ Package into a reusable parameterized harness ④ Register in a central prompt registry
- **Headline benefits/KPIs:** % of key work procedures captured as prompt/harness assets ↑; harness reuse rate across lines/teams ↑; % of prompts with explicit judgment criteria + output contract ↑
- **One-line roadmap:** Short: capture top work procedures as structured prompts → Mid: standardize prompt anatomy template + package as reusable harnesses → Long: shared cross-team/affiliate harness library

> **Note on other disciplines:** Prompts and harnesses also need versioning, metadata cataloging, dependency/lineage tracking, quality gates, and security controls — but these are **generic data-asset disciplines** covered in A-2 (Metadata), C-3 (Lineage), C-2 (Quality), and F-4 (Security). This guide focuses only on what is unique to prompts and harnesses as a content type.

---

## 2. Why? — Why prompt/harness asset management is needed

### [Slide 2] A prompt encodes a work procedure + judgment criteria — that is what makes it special

**👉 One-line summary:** A prompt is not just a command; it encodes two things that previously lived only in an expert's head — a work procedure and judgment criteria — and that is exactly why it needs to be treated differently from any other data artifact.

**Headline:** When a senior inspector writes a defect-classification prompt, they are not issuing a technical command — they are depositing 15 years of judgment into a text file. That deposit is precious, fragile, and easy to lose.

**What a prompt/harness actually encodes:**

A SQL query retrieves data. A script transforms it. A prompt does something different: it encodes a human expert's **work procedure** and **judgment criteria** — the accumulated reasoning that the expert applies when they evaluate a situation and make a call. This is what makes prompts a fundamentally new type of organizational artifact.

| What is encoded | Example in a casting inspection prompt |
|---|---|
| **Work procedure** | "First check crack depth; then check surface porosity; then check zone classification" |
| **Judgment criteria** | "Cracks > 2 mm: REJECT. Porosity > 3 mm diameter: REWORK. Zone A load-bearing: REJECT regardless of size." |
| **Guardrails / forbidden actions** | "Never PASS a Zone A component with any crack, regardless of how small." |
| **Output contract** | "Return: {judgment: PASS/REWORK/REJECT, reason: string, confidence: low/medium/high}" |
| **Grounding context** | "Apply Doosan casting standard QC-12, revision 2025-Q4." |

Because of this encoding, **a prompt is far more valuable than a script** — and far more fragile when left unmanaged. The judgment criteria especially are tacit: the expert may not even be fully conscious of them, and eliciting them correctly is the hardest part of building a good prompt.

**The three stages of maturity — and where most organizations are stuck:**

| Stage | State | What it means |
|---|---|---|
| 1. Tacit | In the expert's head | Not transferable, not scalable, not auditable |
| 2. Ad-hoc prompt | Written text, unmanaged | Explicit but missing anatomy — no criteria, no guardrails, no contract |
| **3. Managed asset** | **Structured anatomy, registered, reusable** | **Reproducible, governed, reusable across teams** |

Most organizations believe they are at Stage 3 because the prompt is "written down." They are at Stage 2. Stage 2 prompts commonly lack explicit judgment criteria (the expert's implicit rules were never drawn out), have no output contract, have no guardrails, and cannot be reused across lines or teams without ad-hoc copying. Stage 3 is the goal of D-3.

🏭 **Manufacturing example:** A senior quality engineer writes a defect-classification prompt from memory and saves it in her laptop. It works well. But it has no judgment criteria written out explicitly — they are implicit in her phrasing. A colleague copies the prompt for a different casting line, adjusts the wording slightly, and the implicit criteria shift. When she retires, no one knows what the real decision rules were. That is Stage 2. Stage 3 would have elicited the criteria explicitly ("Zone A: REJECT regardless, criterion from QC-12 §4.3"), written them into the prompt anatomy, and stored the prompt in a registry with her name as owner.

> ▸ **Backup:** [Backup 2-1] Three-stage comparison table (full detail across all dimensions)

#### [Backup 2-1] Three-stage comparison (detail)

| Dimension | Stage 1: Tacit | Stage 2: Ad-hoc Prompt | Stage 3: Managed Asset |
|---|---|---|---|
| Location | Expert's mind | Personal file, Slack, shared doc | Central prompt registry |
| Judgment criteria explicit? | No | Partially — buried in phrasing | Yes — written out as numbered, testable criteria |
| Guardrails / forbidden actions | Implicit | Usually missing | Explicitly stated in system message |
| Output contract | Implicit (expert knows) | Usually free text | Defined schema: fields, types, enums |
| Few-shot examples | None | Sometimes | Validated golden examples attached |
| Owner | Implicit (the expert) | None beyond personal file | Named steward with accountability |
| Reuse | Verbal transfer / copy-paste | Copy-paste (creates ad-hoc variants) | Registry fetch; harness packaging |
| Survivability | Disappears when expert leaves | May survive but loses fidelity | Organization owns the asset |
| Prompt anatomy completeness | N/A | Incomplete | All seven anatomy components present |

---

### [Slide 3] Five failure modes of unmanaged prompts

**👉 One-line summary:** When prompts are not managed as structured assets, five predictable failures occur — and the most damaging ones are unique to prompts: vague judgment criteria, missing guardrails, no output contract, unreusable one-off logic, and expert knowledge loss.

**Headline:** The failures of unmanaged prompts are not generic data problems — they are specifically about the judgment logic inside the prompt decaying, drifting, or disappearing.

**Failure 1 — Vague or implicit judgment criteria**
The most important failure. When a prompt's judgment criteria are not written out explicitly, the AI makes up its own interpretation. Two colleagues running "the same" prompt on different machines may get different results because the criteria were never pinned down. [src-053]

🏭 **Manufacturing example:** "Reject if the defect looks significant" is not a criterion. "Reject if crack depth > 2 mm, or if defect is in Zone A regardless of size" is. The difference between a prompt that encodes tacit feel and one that encodes explicit rules is the difference between a reproducible asset and a black box.

**Failure 2 — Missing guardrails / forbidden actions**
Judgment prompts without explicit forbidden actions (guardrails) will, under certain inputs, make decisions that the expert would never make — because the boundary conditions were never written down. In a quality inspection context, this can lead to safety-critical escapes.

**Failure 3 — No output contract**
When a prompt's output is free text rather than a defined schema, downstream systems cannot reliably parse it. Each consumer writes its own ad-hoc parsing — making the prompt non-reusable across teams without custom integration work.

**Failure 4 — Hard-coded one-off logic that cannot be reused**
A prompt written for one line, one product, or one team — with those specifics baked directly into the instruction text — cannot be reused across other lines, products, or teams without rewriting it from scratch. This forces every team to re-elicit the expert, re-write the prompt, and re-validate it — wasting the capture effort that was already done.

**Failure 5 — Expert knowledge loss**
When the expert who built the prompt leaves, the prompt's implicit knowledge goes with them. The prompt may still run, but no one can validate whether it still correctly captures the expert's procedure or update it confidently. "When a 35-year veteran retires, up to 90% of what makes them invaluable doesn't follow them out the door." [src-055]

> ▸ **Backup:** [Backup 3-1] Failure mode detail table with cause-effect chains and manufacturing scenarios

#### [Backup 3-1] Failure mode detail

| Failure mode | Unique to prompts? | Root cause | Effect | Manufacturing scenario |
|---|---|---|---|---|
| **Vague judgment criteria** | Yes — prompt-specific | Criteria not elicited explicitly; implicit in expert's phrasing | AI applies arbitrary interpretation; results diverge across runs/teams | "Reject if significant" vs "Reject if crack > 2mm or Zone A" |
| **Missing guardrails** | Yes — prompt-specific | Forbidden actions never captured | AI makes decisions the expert would forbid under edge inputs | Safety-critical Zone A component passed because defect was small |
| **No output contract** | Yes — prompt-specific | Output format not defined | Cannot reuse across teams; every consumer writes ad-hoc parsing | Each plant's integration code parses output differently; field names diverge |
| **One-off non-reusable harness** | Yes — harness-specific | Parameters hard-coded; no parameterization | Must rewrite for every new line/product/team | Casting Line 3 harness cannot be applied to Line 5 without full rewrite |
| **Expert knowledge loss** | Yes — prompt-specific | Prompt stored personally; no structured knowledge transfer | Judgment disappears when expert leaves | Senior inspector retires; no one can confidently update her prompt |
| **Copy-paste drift** | Shared with other assets | Informal sharing; no registry | Silent divergence across teams; one plant runs outdated criteria | 4 plants, 4 diverging prompt versions after shared Slack copy |
| **No reproducibility** | Shared with other assets | No versioning | Past AI decisions cannot be traced | Audit: "which version accepted batch #4471?" — no answer |

---

### [Slide 4] Key objectives — what D-3 sets out to achieve

**👉 One-line summary:** D-3 has five concrete objectives — each one directly addressing a failure mode that is specific to prompts and harnesses as a content type.

**Headline:** The goals of D-3 are: Capture expert judgment explicitly, Build a complete prompt anatomy, Package as a reusable harness, Register for discovery and reuse, and Maintain fidelity as procedures evolve.

1. **Capture tacit expert judgment as explicit, structured anatomy** — Elicit work procedures and judgment criteria from SMEs; decompose them into the prompt's anatomical fields so the rules are testable and the knowledge is owned by the organization, not the individual. (Addresses: Vague criteria, Expert knowledge loss)

2. **Build complete prompt anatomy — criteria, guardrails, output contract, examples** — Every prompt must have all seven anatomical components before it is registered. A prompt missing its guardrails or output contract is incomplete, regardless of whether its instruction text is clever. (Addresses: Missing guardrails, No output contract)

3. **Package the most-used flows as reusable, parameterized harnesses** — Bundle validated prompts + input-data spec + tool sequence + fallback logic into a parameterized harness that runs the same work procedure across different lines, products, and teams without rewriting. (Addresses: One-off non-reusable logic)

4. **Register in a central prompt registry for discovery and reuse** — Replace informal sharing (Slack, email, copy-paste) with a registry-based mechanism. Any team needing a work procedure should find an existing prompt or harness first, not re-elicit from scratch. (Addresses: Copy-paste drift, Knowledge loss)

5. **Maintain judgment-logic fidelity as procedures evolve** — As domain conditions change (standards update, new edge cases emerge), the prompt's judgment criteria and guardrails must be updated to remain faithful to the current procedure — not the procedure from two years ago. (Addresses: Criteria drift, Stale guardrails)

> **Note:** Prompts and harnesses also require versioning, metadata, dependency/lineage tracking, quality gates, and security — but those are generic data-asset disciplines covered in A-2, C-3, C-2, and F-4 respectively. This guide focuses on the five objectives that are unique to prompts and harnesses as a content type.

🏭 **Manufacturing example:** With D-3 in place, a quality engineer's casting-inspection judgment prompt is formally elicited (criteria, guardrails, contract all explicit), registered with her name as owner, packaged into a harness usable across all casting lines with different parameters, and stored in a registry. When she retires, the organization owns the procedure. When the casting standard is updated, the prompt's judgment criteria section is reviewed and updated to match. The knowledge lives in the organization, not in one person's laptop.

---

## 3. When? — When prompt/harness asset management is needed

### [Slide 5] Triggers — when the threshold is crossed

**👉 One-line summary:** Prompt/harness asset management is needed from the moment a prompt moves out of personal experimentation — the key signal is whether the prompt encodes judgment that must remain faithful to a real procedure over time or across people.

**Headline:** The threshold question is not "how complex is this prompt?" but "does this prompt encode judgment that must stay faithful to a real work procedure?" If yes, D-3 applies.

**Always needed:**
- A prompt encodes domain expert judgment that the organization relies on for real decisions (quality calls, scheduling, finance, customer interactions)
- Multiple people or teams are expected to use the same prompt — ad-hoc copying will cause judgment criteria to diverge
- The AI result may need to be explained, reproduced, or audited
- The expert who built the prompt may leave the organization
- The underlying procedure, standard, or terminology the prompt references may change — the prompt's criteria must stay synchronized

**Lighter weight / not yet needed:**
- Pure one-off explorations: a prompt used once personally, with no intention to reuse
- Short-horizon prototypes where the prompt will be discarded or fully rebuilt

**The key question:** *Does this prompt encode judgment that the organization relies on — and must that judgment stay faithful to a procedure over time and across people?* If yes — D-3 applies.

🏭 **Manufacturing example:** A personal AI session asking "summarize this quality report for me" — no governance needed. A prompt that encodes "PASS / REWORK / REJECT criteria from QC-12, used by three shifts, six plants, and shared with Tier-1 suppliers" — D-3 is mandatory immediately.

| Trigger | Why it forces D-3 |
|---|---|
| Prompt encodes expert judgment the org relies on | Judgment must be explicit, not implicit in phrasing |
| Second person will use the prompt | Copy without explicit criteria = silent divergence |
| AI output feeds a real decision | Judgment criteria must be auditable |
| Expert who built it may leave | Knowledge transfer requires explicit anatomy |
| Underlying standard/procedure may change | Criteria must be updated; must know what they reference |
| Same workflow needed across multiple lines/teams | Must be parameterized as a reusable harness |

---

## 4. What? — What prompt/harness asset management is

### [Slide 6] Section agenda — what this section covers

**👉 One-line summary:** This section builds a complete picture of what D-3 is — the anatomy of a prompt as a content type, what a harness is and how it differs, how to parameterize for reuse, the lifecycle, key platforms, and must-know vocabulary.

This section answers:
1. What exactly IS the anatomy of a prompt asset — what components does it have?
2. What is a harness and how does it differ from a single prompt?
3. How does parameterization make a harness reusable rather than one-off?
4. What does the lifecycle of a prompt/harness asset look like?
5. What standards and platforms exist for managing these assets?
6. What are the 10 key terms every practitioner must know?

---

### [Slide 7] The anatomy of a prompt asset — seven components

**👉 One-line summary:** A prompt asset has seven anatomical components — and the most important ones (judgment criteria, guardrails, output contract) are the ones most commonly missing from ad-hoc prompts.

**Headline:** A prompt asset is not just an instruction text — it is a structured document with seven components, each playing a specific role, and each needing to be consciously designed and explicitly written.

> ⚠️ **Scope boundary:** This is about managing the prompt/harness as a structured, governed data asset — NOT about how the AI agent thinks or how to build one. The AI agent is ONLY the **consumer** that executes the asset at runtime. We manage the asset; we do not manage the agent.

A well-formed prompt asset contains these seven components:

| Component | What it captures | Why it matters |
|---|---|---|
| **① Role / persona** | What role the AI is asked to play | Frames the tone and domain knowledge expected |
| **② Task instruction** | What the AI must do, step by step | The explicit work procedure |
| **③ Context / grounding inputs** | What reference material or standards to apply | Makes judgment criteria verifiable and traceable |
| **④ Judgment criteria + decision rules** | The explicit thresholds, rules, and exception-handling logic | **The heart of the prompt** — encodes the expert's actual criteria |
| **⑤ Output contract / format** | Exactly what fields the AI must return, with types and enums | Makes the prompt mechanically reusable — consumers parse reliably |
| **⑥ Guardrails / forbidden actions** | What the AI must NEVER do, regardless of other conditions | Safety boundary — the expert's non-negotiable exceptions |
| **⑦ Few-shot examples** | 3–7 confirmed correct input→output pairs | Calibrates the AI to the expert's specific judgment level |

**The most commonly missing components in ad-hoc prompts are ④, ⑤, and ⑥.** A prompt that says "classify this defect" without explicit criteria (④), without a defined output schema (⑤), and without forbidden actions for edge cases (⑥) is at best Stage 2 — it may work for the expert who wrote it, but it cannot be reliably reused, validated, or governed.

🏭 **Fully worked example — Casting Surface-Defect Judgment Prompt (anatomy walkthrough):**

```
① ROLE / PERSONA:
   "You are a quality inspection assistant trained on Doosan casting standards."

② TASK INSTRUCTION:
   "Evaluate the surface defect described below. Follow this decision procedure:
    Step 1: Check defect type (crack, porosity, scratch).
    Step 2: Measure severity against the QC-12 thresholds.
    Step 3: Check zone classification (Zone A = load-bearing; Zones B-D = non-load-bearing).
    Step 4: Apply the output rules below."

③ CONTEXT / GROUNDING:
   "Apply Doosan Casting Quality Standard QC-12, revision 2025-Q4.
    Zone classification per engineering drawing rev. ENG-2024-07."

④ JUDGMENT CRITERIA + DECISION RULES:
   "Decision rules:
    - Crack depth > 2 mm anywhere → REJECT
    - Surface porosity diameter > 3 mm → REWORK
    - Scratch depth < 0.5 mm → PASS
    - ANY defect in Zone A (load-bearing), regardless of size → REJECT
    - Borderline cases (within 10% of threshold): apply conservative judgment → REWORK if uncertain"

⑤ OUTPUT CONTRACT:
   "Return exactly this JSON structure, no other text:
    {
      "judgment": "PASS" | "REWORK" | "REJECT",
      "reason": "<one sentence, max 150 chars>",
      "confidence": "low" | "medium" | "high",
      "zone": "<zone code from input>"
    }"

⑥ GUARDRAILS / FORBIDDEN ACTIONS:
   "NEVER output PASS for Zone A components with any crack, regardless of size.
    NEVER skip the zone check — if zone is not provided, return confidence: low and request it.
    NEVER invent measurements not present in the input."

⑦ FEW-SHOT EXAMPLES:
   Input: component_id=C-4471, zone=A, defect="surface crack, estimated 0.8mm depth"
   Output: {"judgment":"REJECT","reason":"Zone A component — any crack is reject regardless of depth","confidence":"high","zone":"A"}

   Input: component_id=C-4472, zone=C, defect="shallow scratch, 0.2mm, edge area"
   Output: {"judgment":"PASS","reason":"Scratch below 0.5mm threshold, non-load-bearing zone","confidence":"high","zone":"C"}

   Input: component_id=C-4473, zone=B, defect="porosity, approximately 2.8mm diameter"
   Output: {"judgment":"REWORK","reason":"Borderline porosity (within 10% of 3mm threshold) — conservative judgment applied","confidence":"medium","zone":"B"}
```

**Note — what the prompt asset record adds:** The prompt text above is surrounded by a metadata record (name, version, owner, status, execution settings, dependency map) managed in the registry. Those are governed by the generic data-asset disciplines (A-2, C-3). What D-3 owns is the **prompt anatomy itself** — making sure components ④, ⑤, and ⑥ are explicit and complete.

> ▸ **Backup:** [Backup 7-1] Full prompt asset YAML schema / [Backup 7-2] Data governance ↔ Prompt governance parallel

#### [Backup 7-1] Full prompt asset YAML schema

```yaml
name:           judge.quality-control.casting-defect.v2
version:        2                        # immutable integer; each save = new version
status:         production               # draft | review | staging | production | deprecated
owner:          kenji.tanaka@doosan.com
commit_message: "Updated Zone A rule to unconditional REJECT; added borderline 10% rule per QC-12 v3.3"
tags:           [quality, casting, inspection, judge]
task_domain:    quality-control
intent:         judge

# --- THE SEVEN ANATOMY COMPONENTS ---
template:
  - role: system
    content: |
      You are a quality inspection assistant trained on Doosan casting standards.
      Apply QC-12 rev 2025-Q4 and zone map ENG-2024-07.
      JUDGMENT CRITERIA:
        Crack > 2mm depth         → REJECT
        Porosity > 3mm diameter   → REWORK
        Scratch < 0.5mm           → PASS
        Zone A, any defect        → REJECT (unconditional)
        Borderline (within 10%)   → REWORK (conservative)
      FORBIDDEN: Never PASS a Zone A component with any crack.
      Never invent measurements. Return JSON only.

  - role: user
    content: |
      Component: {{component_id}}, Zone: {{zone_code}}
      Defect: {{defect_description}}
      → Return: {judgment, reason, confidence, zone}

input_variables:
  component_id:       {type: string, description: "Part serial from MES"}
  defect_description: {type: string, description: "Free text from inspector or optical system"}
  zone_code:          {type: string, enum: [A, B, C, D]}

output_format:
  judgment:   {type: string, enum: [PASS, REWORK, REJECT]}
  reason:     {type: string, max_length: 150}
  confidence: {type: string, enum: [low, medium, high]}
  zone:       {type: string}

execution_settings:
  model:       claude-3-5-sonnet-20241022
  temperature: 0.2         # low = deterministic judgment

dependencies:
  data_sources:   [doosan-qc-12-2025-Q4, zone-map-eng-2024-07]
  glossary_terms: [surface-porosity, crack-depth, zone-A]
  eval_datasets:  [casting-inspection-golden-v3]   # E-3 reference

created_at:  2026-03-15
updated_at:  2026-05-20
reviewed_by: quality-governance-council
approved_for: [casting-line-3, casting-line-5]
```

#### [Backup 7-2] Data governance ↔ Prompt governance parallel

As Bojan Ciric (Technology Fellow, Deloitte) writes: "Much like data, prompts represent valuable assets that require effective governance to maximize their utility, ensure compliance with organizational policies, and minimize costs." [src-054]

| Data governance concept | Prompt governance equivalent |
|---|---|
| Data Catalog | Prompt Registry |
| Data Steward | Prompt Steward (prompt owner) |
| Data Asset Record | Prompt Asset Record (versioned YAML/JSON) |
| Metadata schema | Prompt anatomy schema (the seven components) |
| Data lifecycle (active / deprecated / archived) | Prompt lifecycle (draft / staging / production / deprecated / retired) |
| Access control | RBAC on prompt registry (viewer / contributor / domain owner / admin) |
| Change control / approval workflow | Prompt change workflow (SME review → domain owner approval → production) |

---

### [Slide 8] The harness — packaging a prompt into a reusable work unit

**👉 One-line summary:** A harness is the reusable packaging of a prompt (or prompt sequence) together with the input-data specification, tool sequence, output schema, fallback logic, and eval hook — making the full work procedure executable and reusable without rewriting.

**Headline:** If a prompt is the recipe, the harness is the complete kitchen setup: what ingredients arrive (input spec), in what order they are prepared (tool sequence), what the dish looks like (output schema), and what to do if the oven fails (fallback). The harness makes the recipe actually executable.

**What a harness is — and what it differs from a single prompt:**

A single prompt encodes one judgment step. A harness packages:
1. **One or more prompt assets** (referenced by name + version — not copied in)
2. **Input-data specification** — what data must be present before the harness can run, and where it comes from
3. **Tool-call sequence** — the ordered steps: which system to query, which prompt to run, what to write back (tool specs live in D-2 — harness only references them)
4. **Output schema** — the structure delivered to downstream systems
5. **Fallback logic** — what to do when the AI returns low confidence or an output parse error: escalate to human, return a safe default, log and alert
6. **Evaluation hook** — which golden eval set to run against after execution (references E-3)

**Why harnesses matter — the parameterization insight:**

A one-off harness hard-codes the line number, product family, or plant code directly into the execution logic. A reusable harness **parameterizes** those values so the same harness runs across Line 3, Line 5, and Line 7 without rewriting. The judgment criteria stay in the prompt (governed separately); the execution wiring stays in the harness.

🏭 **Fully worked example — Casting Defect Triage Harness:**

```
harness_id:   CastingDefectTriageHarness
version:      v1.0
description:  "Complete work template for triaging surface defects on any casting line.
               Parameterized by line_id. Bundles inspection judgment + MES retrieval + report drafting."
parameters:
  line_id:    string    ← makes this harness reusable across lines without rewriting

prompt_refs:
  - QualityInspectionJudgment @ v2.0   (judgment step)
  - QualityReportDrafting     @ v1.0   (report step)

input_data_spec:
  source:          MES-casting-{{line_id}}          ← parameterized
  required_fields: [component_id, defect_photo_url, inspector_notes, zone_code]

tool_call_sequence:
  Step 1 → optical-inspection-api   (retrieve defect measurements)
  Step 2 → zone-map-lookup          (identify zone from component_id)
  Step 3 → QualityInspectionJudgment@v2  (apply judgment criteria)
  Step 4 → QualityReportDrafting@v1      (generate QMS report)
  Step 5 → MES-write-api (line={{line_id}})   (write result to MES)

output_schema:
  judgment:  PASS | REWORK | REJECT
  report_id: string
  written_to: MES-casting-{{line_id}}

eval_hook:    casting-inspection-golden-v3   (E-3 reference)
  acceptance: precision on REJECT ≥ 0.98, recall on REJECT ≥ 0.95

fallback:
  if confidence < 0.7  → escalate to human reviewer
  if output parse error → log, alert, return status="HOLD"

owner:        kenji.tanaka@doosan.com
approved_for: [casting-line-3, casting-line-5, casting-line-7]
```

**The reuse dividend:** Because `line_id` is a parameter, deploying this harness to Line 7 requires no rewrite — only adding `casting-line-7` to `approved_for`. The judgment logic (in the prompt asset) is governed separately and shared by all lines.

> ▸ **Backup:** [Backup 8-1] Prompt vs. Harness comparison table / [Backup 8-2] Harness asset full schema

#### [Backup 8-1] Prompt vs. Harness — key comparison

| Dimension | Prompt Asset | Harness Asset |
|---|---|---|
| Scope | Single judgment/instruction step | Complete work-procedure unit (multi-step) |
| Contents | Seven anatomy components (role, task, context, criteria, output contract, guardrails, examples) | Prompt refs + input spec + tool sequence + output schema + fallback + eval hook |
| Granularity | One cognitive step | One full workflow (e.g., retrieve → judge → report → write-back) |
| Reusability | Referenced by multiple harnesses | Reused as a complete job package; parameterized for different lines/teams |
| Hard-coded vs. parameterized | Criteria are explicit but context-specific | Parameters (line_id, product_family) make the harness context-independent |
| Versioning trigger | Any change to judgment criteria, guardrails, output contract, or examples | MAJOR: any constituent prompt changes its output schema; input/output schema changes |
| Ownership | Domain expert who owns the procedure | Team accountable for the full workflow |

#### [Backup 8-2] Harness asset full schema

| Field | Type | Description |
|---|---|---|
| `harness_id` / `name` | string | Unique identifier |
| `version` | semver | Version of the harness package |
| `parameters` | dict of {name, type} | Parameterizable values that make the harness reusable |
| `prompt_refs` | list of {prompt_id, version, role} | Constituent prompt assets by exact version |
| `input_data_spec` | {source, schema_ref, required_fields} | What must be present before harness can run |
| `tool_call_sequence` | ordered list of {step, tool/prompt, purpose} | Execution order (tool specs reference D-2) |
| `output_schema` | {to_system, fields} | Output delivered to downstream system |
| `eval_hook` | {dataset_ref, acceptance_criteria} | E-3 eval dataset + pass/fail thresholds |
| `fallback_behavior` | structured text | Response to low confidence / parse error |
| `owner` | string | Accountable team/individual |
| `approved_for` | list | Authorized scopes (can be extended without rewrite via parameterization) |
| `created_at` / `updated_at` | timestamps | Lifecycle timestamps |

---

### [Slide 9] The prompt/harness asset lifecycle — five stages

**👉 One-line summary:** A prompt/harness asset goes through five ordered stages from birth to retirement — Capture, Structure, Register, Govern, and Retire/Evolve — each producing a specific output that feeds the next.

**Headline:** The lifecycle is not a one-time setup; it is a continuous loop — because work procedures evolve, standards are updated, and new edge cases emerge that the original prompt didn't cover.

```
[1. CAPTURE] → [2. STRUCTURE] → [3. REGISTER] → [4. GOVERN] → [5. RETIRE/EVOLVE]
     ↑                                                                    |
     └──────── Feedback: new edge cases, standard changes, expert review ─┘
```

**Stage 1 — CAPTURE:** Elicit the expert's work procedure and judgment criteria. Output: a raw but complete description of inputs, decision steps, criteria, output format, forbidden actions, and example cases.

**Stage 2 — STRUCTURE:** Decompose the raw description into the seven anatomy components. Write explicit criteria and guardrails. Define the output contract. Collect golden examples. Output: a structured, complete prompt draft.

**Stage 3 — REGISTER:** Attach metadata, validate against golden eval cases, assign version, publish to the registry. Note: versioning, metadata management, and dependency/lineage tracking at this stage are generic data-asset disciplines (A-2, C-3) — D-3 focus is ensuring the anatomy is complete before registration.

**Stage 4 — GOVERN:** Maintain the asset through production life — update judgment criteria when procedures change, update guardrails when new edge cases emerge, re-validate against golden cases. D-3's specific governance question: *does the prompt still faithfully encode the current expert procedure?*

**Stage 5 — RETIRE/EVOLVE:** When the procedure is superseded or the domain conditions change fundamentally, mark as deprecated with a migration notice. The underlying knowledge should be re-elicited into a new version, not silently discarded.

🏭 **Manufacturing parallel:** This mirrors how a plant manages SOPs: draft → reviewed → approved → released → revision-controlled as conditions change → superseded with archive retention. The key D-3 question at each review: "Do the judgment criteria still reflect how the expert would evaluate this today?"

> ▸ **Backup:** [Backup 9-1] Per-stage deliverables, prerequisites, and responsible roles

#### [Backup 9-1] Lifecycle stage detail

| Stage | Key activity | D-3-specific deliverable | Generic data-asset work (see other topics) | Responsible role |
|---|---|---|---|---|
| 1. Capture | SME interview; think-aloud; AI-assisted transcription | Raw procedure: inputs, judgment steps, criteria, forbidden actions, examples | — | Prompt Engineer + SME |
| 2. Structure | Fill all seven anatomy components; define output schema; collect golden examples | Complete prompt anatomy draft | Terminology alignment with glossary (A-2, D-x) | Prompt Engineer / Data Steward |
| 3. Register | Validate prompt anatomy completeness; run golden eval; publish to registry | Prompt with all seven components present + passing eval | Metadata attach, dependency map, version ID (A-2, C-3) | Prompt Engineer + Domain Owner |
| 4. Govern | Review judgment criteria vs. current procedure; update guardrails for new edge cases | Updated criteria + guardrails in new version with commit message explaining WHY | Change approval workflow, version increment (generic governance) | Steward + Domain Owner (ongoing) |
| 5. Retire/Evolve | Re-elicit if procedure changed fundamentally; mark deprecated with migration | New version with updated procedure, or deprecated record | Archive, retention, audit trail (generic) | Domain Owner + Governance Council |

---

### [Slide 10] Standards and platforms — what is available

**👉 One-line summary:** No single global standard yet mandates a specific prompt-asset schema, but the industry has converged on a practical baseline — and AWS has declared prompt template management a "Well-Architected best practice," meaning it is now expected in any production AI system.

**Headline:** Prompt asset management has moved from experimental to production standard — the key question is no longer "should we do this?" but "which platform fits our stack?"

**Why a standard approach matters for prompt anatomy:**
Without a shared template and platform, every team invents its own format for judgment criteria and guardrails — some teams bury criteria in the middle of a paragraph, others skip them entirely. A standard anatomy template makes it enforceable that every registered prompt has all seven components present.

**The emerging baseline:**
- MLflow Prompt Registry (open source, Linux Foundation): versioned, named assets; `register_prompt()` / `load_prompt()` pattern
- Langfuse (open source, self-hostable): immutable versions + mutable labels (production/staging/latest); SDK caching
- AWS Well-Architected Generative AI Lens, Best Practice GENOPS03-BP01: "Implement prompt template management" — officially in AWS production AI guidelines since November 2024 [src-110]
- BAML (BoundaryML): a file-format DSL where schema and prompt co-exist in the same file — prevents anatomy drift between the prompt text and its output contract

🏭 **Manufacturing parallel:** Prompt asset management is to AI systems what ISO 9001 document control is to quality management — not one optional tool, but a category of discipline that any production-grade AI implementation is now expected to practice.

> ▸ **Backup:** [Backup 10-1] Prompt management platform comparison table

#### [Backup 10-1] Prompt management platform comparison

Note: Humanloop shut down September 8, 2025 [src-103]. All others below are active as of 2026-06-09.

| Platform | Vendor / License | Key Asset-Management Features |
|---|---|---|
| **MLflow Prompt Registry** | Linux Foundation (open source) | Immutable versions; commit messages; key-value tags; mutable aliases (production/staging); model_config versioned with prompt; diff view; `search_prompts` API [src-105] |
| **Langfuse** | Langfuse GmbH (open source, self-hostable) | Immutable integer versions; mutable labels; protected labels (admin-locked); SDK caching; prompt composability (reference other prompts); link traces to prompt versions [src-100] |
| **LangSmith Prompt Hub** | LangChain (cloud) | Commit hashes; commit tags as named aliases; environment management (staging/production); webhook triggers; owner access control [src-160] |
| **Agenta** | Agenta AI (MIT open source) | Git-like branching (variants); version history; comparison mode (50+ models); RBAC + SSO + audit logs; developer + non-developer collaboration [src-161] |
| **PromptLayer** | PromptLayer (cloud) | Template registry; release labels; commit messages; A/B testing via Dynamic Release Labels; cost + latency per version; scheduled regression tests [src-104] |
| **LangWatch** | LangWatch (cloud, ISO 27001/SOC 2) | Immutable version IDs; CLI YAML-in-Git workflow; dev/staging/prod separation; evaluation tied to prompt versions [src-109] |
| **Amazon Bedrock Prompt Management** | AWS (cloud, GA Nov 2024) | Version control; evaluation; environment separation; CI/CD integration; IAM + CloudTrail governance; Well-Architected best practice GENOPS03-BP01 [src-110] |
| **BAML** | BoundaryML (open source DSL) | Schema + prompt co-location (no anatomy drift); static analysis/linting; validates LLM output against schema at parse time; Git-diffable; transpiles to Python/TS/Go/Java/Ruby/Rust [src-111] |

---

### [Slide 11] Key glossary — 10 terms every practitioner must know

**👉 One-line summary:** Ten terms form the working vocabulary for prompt/harness asset management — each grounded in a manufacturing analogy so the concept transfers immediately to a shop-floor context.

| Term | Easy one-line definition | Manufacturing analogy |
|---|---|---|
| **Prompt Asset** | A prompt managed as a versioned, governed record with all seven anatomy components — not a raw text string | A revision-controlled SOP with explicit judgment criteria, approval records, and change history |
| **Prompt Anatomy** | The seven structural components every prompt must have: role, task, context, judgment criteria, output contract, guardrails, examples | The standard form sections every work instruction must fill in: purpose, scope, tools, steps, acceptance criteria, safety notes, worked examples |
| **Judgment Criteria** | The explicit thresholds, rules, and exception-handling logic the expert applies — the heart of the prompt | Acceptance criteria in a quality specification: the exact measurements and conditions that determine pass/fail |
| **Guardrails / Forbidden Actions** | The non-negotiable boundaries the AI must never cross, regardless of other conditions | Safety interlocks in a manufacturing process: rules that cannot be overridden no matter what the measurements say |
| **Output Contract** | The defined schema of what the prompt must return — fields, types, enums — enabling reliable downstream parsing | Bill-of-delivery specification: exactly what the process must output for the next station to accept it |
| **Harness** | A packaged work template bundling prompt refs + input spec + tool sequence + output schema + fallback logic | A complete job package: work instruction + BOM + equipment setup sheet + QA criteria |
| **Parameterized Harness** | A harness where line-specific or team-specific values are parameters, making the same harness reusable across contexts | A standard-work template with variable fields for line number or product type — fill in the variables, run the same procedure |
| **Prompt Registry** | The central, searchable repository where prompt assets are stored, versioned, and fetched at runtime | A controlled document management system (DMS) for all active work instructions |
| **Golden Examples** | 3–7 confirmed correct input→output pairs from historical cases, used to calibrate the prompt and test new versions | Golden samples in quality control: the approved reference pieces against which new production is compared |
| **PromptOps** | The operational discipline of governing, testing, and maintaining prompts as production assets — analogous to DevOps for software | Total Productive Maintenance (TPM) applied to AI instructions: scheduled review, quality tracking, ownership accountability |

---

## 5. How? — How to prepare the prompt/harness as a managed data asset for AI

### [Slide 12] Section agenda — what this section covers

**👉 One-line summary:** This section is the practical playbook: how to elicit expert judgment and convert it into a complete prompt anatomy, how to build and parameterize a reusable harness, how to avoid the common pitfalls, and what to automate.

This section answers:
1. How do you elicit tacit work procedure + judgment criteria from an SME?
2. How do you decompose the raw capture into the seven anatomy components?
3. What does a fully worked prompt anatomy look like — built step by step?
4. How do you package validated prompts into a reusable parameterized harness?
5. What are the prompt/harness-specific pitfalls to avoid?
6. What does ongoing operations look like — who governs the library and how often?

---

### [Slide 13] Step 1 — Eliciting tacit judgment: the SME capture session

**👉 One-line summary:** Before writing a single line of a prompt, you must draw the expert's work procedure and judgment criteria out of their head — this is the most overlooked step, and skipping it means the prompt will lack the explicit criteria that make it a managed asset.

**Headline:** You cannot write explicit judgment criteria if you have not first elicited them. Capture comes before structure — and capture requires deliberate techniques, not just "asking the expert to describe what they do."

**Why explicit elicitation is hard:**
Expert judgment is often tacit — the expert applies rules unconsciously without fully articulating them. When asked to describe their process, experts typically describe the *ideal* case, not the edge cases and exceptions that contain most of the real judgment. Research shows: "When a 35-year veteran retires, up to 90% of what makes them invaluable doesn't follow them out the door in a folder of documents." [src-055] The capture session is the moment to extract that 90%.

**Capture techniques:**
- **Structured SME interview:** Ask the expert to walk through a decision scenario step by step. Use the "Think Aloud" technique — ask them to narrate their reasoning while performing the task. [src-166]
- **Case-based elicitation:** Bring 3–5 real historical cases (2–3 standard, 1–2 edge cases, 1 failure case). Ask: "How would you decide this one? What rules are you applying?"
- **AI-assisted extraction:** Conversational AI assistants can guide the SME through the elicitation and structure responses into a first draft in under one minute. [src-166]
- **Forbidden-actions probe:** Explicitly ask: "What would you NEVER do, regardless of what the measurements show?" — this surfaces guardrails.

**What must come out of the capture session (the seven anatomy fields):**

| Anatomy field | Elicitation question |
|---|---|
| Task instruction (procedure steps) | "Walk me through what you do, step by step, from start to finish." |
| Judgment criteria | "What are the exact thresholds or rules you use? What measurements or conditions determine your decision?" |
| Context / grounding | "What documents, standards, or lookup tables do you refer to?" |
| Output format | "What does your final output look like — what do you write or record?" |
| Guardrails | "What would you NEVER do, regardless of other conditions? What are your non-negotiables?" |
| Edge cases / examples | "Show me a clear PASS, a clear REJECT, and a borderline case — and explain why." |
| Input data needed | "What data or information do you need to have in front of you before you start?" |

🏭 **Manufacturing example — quality inspector's verbal description (raw):**
> "When I see surface marks, I first check if they're within the 0.3mm tolerance in document QC-12. If borderline, I check location — edge defects pass, center defects don't. I also check if the part is for a safety-critical assembly — that's an automatic hold regardless of size."

This one paragraph, when elicited carefully, yields: ① crack size threshold from QC-12, ② a spatial rule (edge vs. center), ③ a safety-critical override as a guardrail, ④ the need for part-destination metadata as an input variable, and ⑤ a pointer to the QC-12 document as a dependency. All five of these must be written explicitly into the prompt anatomy.

> ▸ **Backup:** [Backup 13-1] Full SME capture interview guide + question bank

#### [Backup 13-1] Full SME capture interview guide

**Pre-interview setup:**
- Identify the SME whose expertise the prompt will encode
- Prepare 3–5 real historical cases: 2–3 "standard," 1–2 edge cases, 1 failure case if available
- Book 60–90 minutes for the initial session; plan a follow-up validation session

**Interview script (opening):**
> "We are going to document your judgment process so an AI assistant can apply the same criteria consistently. Please walk through this task step by step as if explaining it to a new colleague. Narrate what you look at, what you check, and how you decide. There are no wrong answers — we want to capture exactly how you actually do this, including the exceptions."

**Core elicitation questions:**

| Question | Maps to anatomy field |
|---|---|
| "What is the first thing you look at when you receive this task?" | Input variables (first data element) |
| "What are the criteria you use to classify/judge/decide?" | Judgment criteria |
| "What thresholds or reference numbers do you use?" | Judgment criteria + dependencies.data_sources |
| "Are there conditions where the normal rule doesn't apply?" | Judgment criteria exceptions + guardrails |
| "What would you NEVER do, no matter what the measurements show?" | Guardrails / forbidden actions |
| "Can you show me a clear PASS, a clear REJECT, and a borderline case?" | Golden examples |
| "What documents, tables, or standards do you refer to?" | Dependencies.data_sources + glossary_terms |
| "What is the output you produce — what form does it take?" | Output contract |
| "Who else applies this procedure? Are there situations they do it differently?" | Scope / `approved_for` |

**Post-interview validation:**
- Produce a first-draft structured prompt using the captured information
- Send to the SME: "Does this correctly capture how you would evaluate this scenario?" (give them 3 test cases)
- Revise based on feedback; repeat until SME confirms all criteria are accurate
- Use the validated prompt text as the basis for Stage 2 (Structure)

---

### [Slide 14] Step 2 — Decomposing into the seven anatomy components

**👉 One-line summary:** After capture, the raw expert description is systematically decomposed into the seven anatomy components — placing stable judgment criteria in the system role, variable inputs in the user role, and defining the output contract as a typed schema.

**Headline:** Structure converts expert language into machine-executable anatomy. The key discipline: judgment criteria go in the system message (stable, governance-controlled), variable data goes in the user message (dynamic, parameterized).

**The decomposition decision for each element:**

```
From raw capture →→ Place in prompt anatomy as:

"First check if they're within the 0.3mm tolerance"
  → JUDGMENT CRITERIA (system message): "Crack depth > 0.3mm → REWORK"

"Check if the part is for safety-critical assembly — automatic hold"
  → GUARDRAIL (system message): "NEVER PASS if safety_critical = true, regardless of size"

"QC-12 tolerance document"
  → CONTEXT (system message): "Apply QC-12 rev 2025-Q4" + dependency record

"Component ID, defect description, zone code"
  → INPUT VARIABLES (user message template): {{component_id}}, {{defect_description}}, {{zone_code}}

"Write PASS / REWORK / REJECT with a reason"
  → OUTPUT CONTRACT: {judgment: PASS|REWORK|REJECT, reason: string, confidence: low|medium|high}

"Edge case: edge defects vs. center defects"
  → FEW-SHOT EXAMPLE: provide the borderline case explicitly
```

**The system/user message split — why it matters:**
Placing judgment criteria in the `system` role and variable data in the `user` role is not just good practice — it is the structural separation that makes governance possible. The `system` message is the part that changes rarely, requires approval to change, and is where the expert's criteria live. The `user` message is the part that changes per execution, injecting the specific input data at runtime.

**Terminology alignment:**
Cross-reference all domain terms in the prompt text against the organizational glossary. If the prompt says "defect" but the glossary standard is "nonconformance," align them. Misaligned terminology causes silent mismatches downstream.

**Output contract discipline:**
The output must be a defined schema — not free text, not "return an assessment." A defined schema with enums (`PASS|REWORK|REJECT`) and field types enables:
- Automated parsing by downstream systems
- Automated validation (did the AI comply with the schema?)
- Regression testing against golden cases
- Reuse across teams without integration rework

🏭 **Manufacturing example — full decomposition of the casting inspection capture:**

```
CAPTURE (raw): "I check crack depth against QC-12. If > 2mm: reject. Zone A: always reject
               regardless. Edge defects get lenience. Output: decision, reason, confidence."

DECOMPOSED ANATOMY:

System message (stable criteria — requires approval to change):
  Role: Quality inspection assistant, apply QC-12 rev 2025-Q4
  Judgment criteria: Crack > 2mm → REJECT; Porosity > 3mm → REWORK; Scratch < 0.5mm → PASS
  Zone rule: Zone A any defect → REJECT (unconditional)
  Edge/center: edge defects pass if below threshold; center defects use strict threshold
  Guardrail: NEVER PASS Zone A + any crack; NEVER skip zone check

User message (variable data — injected at runtime):
  Component: {{component_id}}, Zone: {{zone_code}}
  Defect: {{defect_description}}

Input variables: component_id (string), zone_code (enum A/B/C/D), defect_description (string)

Output contract: {judgment: PASS|REWORK|REJECT, reason: string<150, confidence: low|medium|high, zone: string}

Examples: 3 PASS, 2 REWORK, 2 REJECT cases from historical records
```

> ▸ **Backup:** [Backup 14-1] Standardization checklist before registry submission

#### [Backup 14-1] Standardization checklist (before a prompt can enter the registry)

- [ ] All seven anatomy components present: role, task, context, judgment criteria, output contract, guardrails, examples
- [ ] Judgment criteria are explicit and testable: specific thresholds, rules, not vague qualitative descriptions
- [ ] Variable placeholders use consistent syntax (e.g., `{{variable_name}}` — Jinja2 style)
- [ ] Output format is a defined schema with types and enums — NOT free text
- [ ] At least 3 golden examples (correct input→output pairs from historical records): min 1 standard case, 1 edge case, 1 reject/fail case
- [ ] Guardrails explicitly stated: at minimum one forbidden action
- [ ] All domain terms in the prompt cross-checked against organizational glossary
- [ ] No hardcoded secrets, API keys, or PII embedded in the template [src-165]
- [ ] All data sources, standards, and documents the prompt references are listed (for dependency record)
- [ ] Domain owner notified and available for anatomy review

---

### [Slide 15] Step 3 — Building and parameterizing a reusable harness

**👉 One-line summary:** Once individual prompt assets are validated, they are packaged into a harness — and the key design decision is which values to parameterize so the harness can run across different lines, teams, or contexts without rewriting.

**Headline:** The difference between a one-off harness and a reusable one is parameterization: variables for the things that differ by context (line, product, team) while the judgment logic stays in the governed prompt asset.

**The packaging step:**

Once the prompt anatomy is complete and the prompt is validated against golden cases:
1. **Identify the full work unit** — what other steps surround this judgment? What data must be fetched first? What must be written back? What happens if the AI is uncertain?
2. **Write the harness.yaml** — `prompt_refs` (name + version), `input_data_spec`, `tool_call_sequence`, `output_schema`, `fallback_behavior`, `eval_hook`
3. **Identify parameterizable values** — which fields differ by deployment context (line_id, product_family, plant_code)? Make those parameters, not hard-coded values.
4. **Validate end-to-end** — run the complete harness (not just the prompt in isolation) against representative scenarios including the fallback trigger cases
5. **Document the fallback explicitly** — what happens when `confidence < 0.7`? What happens on output parse error? These must be specified, not left to runtime improvisation.

**The parameterization design principle:**
- **In the prompt (not parameterized):** judgment criteria, guardrails, output contract — these are stable, governed by the expert's procedure, shared across all uses of the harness
- **In the harness (parameterized):** line_id, product_family, MES source system — these are context-specific deployment variables

🏭 **Manufacturing example — "Incoming Inspection Judgment Harness" (parameterized for product family):**

```
harness_id:    IncomingInspectionJudgmentHarness
version:       v2.0
parameters:
  product_family: string   ← parameterized: "hydraulic-cylinder", "casting-blank", "precision-shaft"

prompt_refs:
  - IncomingInspectionJudgment @ v2.1    (judgment; criteria cover all three product families)

input_data_spec:
  source:          ERP-incoming-inspection
  required_fields: [material_id, inspection_report_path, part_number, safety_critical_flag, product_family]
  product_family_filter: {{product_family}}

tool_call_sequence:
  Step 1 → tolerance-db-query (get tolerance spec by part_number + product_family)
  Step 2 → material-master-lookup (get part classification)
  Step 3 → IncomingInspectionJudgment@v2.1 (apply judgment)
  Step 4 → MES-write-api (write decision to incoming inspection record)

output_schema:
  decision:          accept | hold | reject
  reason:            string
  confidence:        float
  reviewer_required: boolean

fallback:
  if confidence < 0.7        → reviewer_required = true; decision = "hold"
  if output parse error      → log + alert + return decision = "hold"
  if safety_critical = true and decision = "accept"
                             → override to reviewer_required = true (safety gate)

eval_hook:   incoming-inspection-golden-v1.4   (E-3 reference)
  acceptance: precision on REJECT ≥ 0.97

owner:        incoming-inspection-team@doosan.com
approved_for: [plant-gyeonggi, plant-changwon]   ← easily extended to new plants
```

**The reuse dividend:** Adding a new plant requires only: ① adding the plant to `approved_for` ② confirming the product_family values apply. The judgment logic (in the prompt) stays unchanged and benefits from any improvements made to it over time.

> ▸ **Backup:** [Backup 15-1] Harness construction checklist / [Backup 15-2] Decision: single prompt vs. harness

#### [Backup 15-1] Harness construction checklist

- [ ] All constituent prompt assets are in `production` or `staging` status in the registry
- [ ] All prompt refs specify exact version (not `latest` — harness uses pinned versions for stability)
- [ ] All required input fields are listed and typed; source system is specified
- [ ] Tool-call sequence is complete and in order; tool specs reference D-2 (not embedded here)
- [ ] Fallback logic covers all three failure modes: low confidence, parse error, safety override
- [ ] Output schema defined with types (not free text)
- [ ] Eval hook references a named E-3 golden dataset + acceptance thresholds
- [ ] Parameters identified: all context-varying values are parameters, not hard-coded
- [ ] End-to-end harness tested against representative scenarios (including fallback triggers)
- [ ] Owner assigned (team or individual)

#### [Backup 15-2] Single prompt vs. harness — when to package as a harness

| Situation | Use a single prompt | Package as a harness |
|---|---|---|
| One cognitive judgment step | ✓ | — |
| Multi-step workflow: fetch data → judge → write back | — | ✓ |
| Multiple prompts in sequence | — | ✓ |
| Needs fallback / human escalation logic | — | ✓ |
| Will be deployed to multiple contexts (lines, teams) | — | ✓ (parameterize) |
| Will be reused as a self-contained work unit | — | ✓ |
| Quick one-off exploration | ✓ | — |

---

### [Slide 16] Common pitfalls — the prompt/harness-specific ones

**👉 One-line summary:** The most damaging pitfalls in prompt/harness asset management are the ones unique to this content type: vague criteria, missing guardrails, no output contract, an un-parameterized harness, judgment logic that drifts from the real procedure, and prompts that mix multiple jobs.

**Headline:** Technical infrastructure (registry, versioning) is not enough — the prompt anatomy must also be correct: explicit criteria, hard guardrails, defined output contract, and one-job-per-prompt discipline.

**Pitfall 1 — Vague or implicit judgment criteria (prompt-specific)**
"Evaluate the quality of this component" is not a judgment criterion — it is a task description. Without explicit thresholds and rules, the AI makes up its own interpretation, which differs from run to run and from the expert's actual procedure.
→ **Avoid:** Every judgment prompt must have numbered, testable criteria with specific thresholds (e.g., "Crack > 2mm depth → REJECT"). If the SME says "it depends" without specifying on what — go back to the capture session.

**Pitfall 2 — Missing guardrails / forbidden actions (prompt-specific)**
A prompt without explicit forbidden actions will, on edge cases, do things the expert would never do. In quality control, this can mean passing a safety-critical defect.
→ **Avoid:** Every prompt must have at least one explicit guardrail. Use the question: "What would you NEVER do, regardless of what the measurements show?" — the answer is the guardrail.

**Pitfall 3 — No output contract (prompt-specific)**
Free-text output ("provide an assessment of the defect") means every downstream consumer must write ad-hoc parsing code. The prompt cannot be shared across teams without integration rework.
→ **Avoid:** Every prompt must return a defined schema. For judgment tasks: `{decision: enum, reason: string<N, confidence: enum}` as a minimum.

**Pitfall 4 — Hard-coded one-off harness that cannot be reused (harness-specific)**
A harness with `source: MES-casting-line-3` hard-coded cannot be deployed to Line 5 without a rewrite. Every new context requires rebuilding — losing the investment in validation.
→ **Avoid:** Identify parameterizable values during harness design. Anything that varies by deployment context (line_id, plant_code, product_family) becomes a parameter.

**Pitfall 5 — Judgment logic drifting from the real current procedure (prompt-specific)**
The standard was updated. The expert's tacit knowledge evolved. But the prompt still encodes the old criteria from three years ago — silently.
→ **Avoid:** The governing question at every prompt review is: "Does this prompt still faithfully encode how the expert would evaluate this today?" Review triggers: any update to a referenced standard, any quality-incident review, minimum annual re-validation.

**Pitfall 6 — Over-stuffed prompt mixing multiple jobs (prompt-specific)**
One prompt asked to classify the defect, determine root cause, draft a QMS report, and recommend a corrective action — all at once. The prompt becomes a black box; individual judgment steps cannot be tested, governed, or reused independently.
→ **Avoid:** One prompt, one job. If multiple jobs are needed, chain them in a harness — each prompt does one cognitive step, each can be governed independently.

🏭 **Manufacturing example:** A plant builds one giant prompt: "Read the inspection report, classify the defect, determine root cause, estimate rework cost, and draft the QMS nonconformance report." It works most of the time but fails mysteriously on edge cases — and no one can tell which of the four jobs failed. Separating into four prompts (each with its own criteria and output contract) chained in a harness makes each step independently testable and replaceable.

---

### [Slide 17] Register, govern, and ongoing operations

**👉 One-line summary:** A prompt registry is not a one-time setup — it requires ongoing stewardship: periodic reviews of whether judgment criteria still match the current procedure, re-validation when standards change, and deprecation of prompts that have drifted from their domain.

**Headline:** Managing the prompt library is an ongoing data stewardship responsibility — the key question every governance cycle is not "did the prompt change?" but "does the prompt still faithfully capture the current expert procedure?"

**Registration — the D-3-specific gate before publishing:**
Before a prompt enters the registry, the anatomy must be complete: all seven components present, judgment criteria explicit and testable, output contract defined, at least 3 golden examples validated by the SME, guardrails stated. A prompt that passes a linting check (required metadata fields present) but has vague criteria is still a Stage 2 prompt.

> **Note on generic registry operations:** Versioning every change, assigning labels (production/staging), running automated regression tests, managing dependency alerts, enforcing approval workflows, access control — these are generic data-asset operations described in A-2 (Metadata), C-3 (Lineage), and the general data governance topics. D-3's unique governance question is: *does the prompt's judgment logic still match the expert's current procedure?*

**D-3-specific governance cadence:**

| Frequency | D-3-specific activity |
|---|---|
| On change to any referenced standard or procedure | Re-validate judgment criteria: do they still match the updated standard? |
| After any quality incident involving this prompt | Post-incident review: which criterion failed? Was it missing or wrong? |
| Annually (at minimum) | Re-elicit from SME: "Does this prompt still capture how you'd evaluate this today?" |
| When the expert who built it changes role or leaves | Knowledge transfer: new owner validates all anatomy fields with the incoming expert |
| When deploying to a new context (line, plant) | Scope review: are the criteria, guardrails, and examples appropriate for this context? |

**Access roles (specific to prompt anatomy governance):**

| Role | Can do |
|---|---|
| **SME / Domain Expert** | Validate and sign off that judgment criteria and guardrails are correct |
| **Prompt Engineer / Data Steward** | Write and maintain the prompt anatomy; ensure all seven components are present |
| **Domain Owner** | Approve promotion to production; accountable for criteria fidelity |
| **Registry Viewer** | Discover and fetch prompts for use; cannot modify |

🏭 **Manufacturing example:** The casting quality standard is updated from QC-12 v3.2 to v3.3. The dependency alert fires (via whatever change-tracking mechanism is in place). The prompt owner opens the anatomy: "Updated porosity threshold from 3mm to 2.5mm per QC-12 v3.3, quality lead approval 2026-06-01." The updated prompt passes the golden eval (precision on REJECT still ≥ 0.98), gets domain owner sign-off, and promotes to v2.1. Elapsed time: 2 days. The judgment criteria are now current.

> ▸ **Backup:** [Backup 17-1] Full approval workflow roles and RACI / [Backup 17-2] Automation features for prompt/harness management

#### [Backup 17-1] Approval workflow roles and RACI

| Role | Responsibilities | Capture | Structure | Register | Govern | Retire |
|---|---|---|---|---|---|---|
| **SME** | Validates judgment criteria and guardrails are correct | R | C | I | R (on criteria review) | C |
| **Prompt Engineer / Data Steward** | Fills anatomy fields; ensures all seven components present | A | R | R | R | R |
| **Domain Owner** | Final approval that prompt is fit for production; accountable for criteria fidelity | I | C | A | A | A |
| **Quality Assurance** | Runs golden eval; validates regression tests pass | — | C | R | R | I |
| **Governance Council** | Cross-domain oversight; policy; cross-affiliate releases | — | — | I | C | I |

R = Responsible; A = Accountable; C = Consulted; I = Informed

#### [Backup 17-2] Automation features that help manage prompt/harness assets

Note: these tools manage the **data asset** (the prompt record) — they are not AI-building tools.

| Tool/feature | What it manages | Benefit |
|---|---|---|
| **Prompt anatomy linting** | Checks that all seven components are present; flags missing criteria or guardrails | Enforces completeness before registration |
| **Side-by-side diff view** | Shows exactly what changed in criteria or guardrails between versions | Enables meaningful review; identifies behavioral changes |
| **Protected production labels** | Prevents unauthorized edits to the production-tagged version | Anatomy integrity; access control |
| **Automated regression tests** | Runs golden eval on every version change; blocks promotion if accuracy drops | Quality gate before production (anatomy-level testing) |
| **Dependency change alerts** | Notifies owner when a referenced standard or document is updated | Triggers criteria re-validation |
| **Search and discovery API** | Find prompts by domain, intent, owner, anatomy completeness status | Enables reuse before re-elicitation |

---

## 6. KPI? — Management metrics

### [Slide 18] Five KPIs for managing the prompt/harness asset library

**👉 One-line summary:** Five KPIs measure whether your prompt/harness asset library is actually capturing and maintaining expert knowledge as reusable, governed assets — covering procedure coverage, anatomy completeness, harness reuse, fidelity to procedure, and judgment-logic parameterization.

**Headline:** These KPIs measure asset quality specific to prompts and harnesses — not generic data management metrics and not AI performance. They tell you whether expert judgment has been properly captured, structured, and made reusable.

| KPI | Easy meaning | Measurement purpose | Target direction | Expected effect |
|---|---|---|---|---|
| **Work Procedure Capture Rate** | What % of the organization's identified key work procedures (those relied on for real decisions) have been captured as a registered prompt or harness asset | Without this KPI you cannot know how much tacit judgment is still unmanaged — still "in heads or in laptops" | ↑ Target: 80% of identified key procedures within 18 months | Systematic reduction of knowledge-loss risk; expert judgment survives personnel change |
| **Prompt Anatomy Completeness Rate** | What % of registered prompts in production status have all seven anatomy components present and non-empty (role, task, context, judgment criteria, output contract, guardrails, at least 1 example) | A prompt with a missing output contract or implicit criteria is not a managed asset — it is still Stage 2 | ↑ Target: 100% for production-status prompts | Every production prompt is fully explicit; judgment can be tested, reviewed, and updated |
| **Harness Reuse Rate** | For the most-used work procedures: are they implemented as parameterized harnesses used across multiple lines/teams, or as separately maintained one-off versions per context? (ratio of shared harness executions vs. total executions of that work procedure type) | Low reuse = teams are copy-pasting and locally modifying instead of sharing governed harnesses; reuse rate measures whether parameterization investment is working | ↑ Industry benchmark: 40–60% reuse across departments [src-200] | Reduction in ad-hoc local variants; judgment updates propagate to all users of the harness |
| **Judgment-Fidelity Re-validation Rate** | What % of production prompts whose referenced standards or procedures have been updated in the last 6 months have been re-validated (SME sign-off that criteria still match the current procedure) | This KPI is unique to prompts: it measures whether the judgment logic has kept pace with how the procedure actually works today | ↑ Target: 100% within 30 days of any dependency change | Prompts do not silently encode outdated decision criteria |
| **SME-Validated Procedure Coverage** | What % of production prompts have a documented SME sign-off that the judgment criteria and guardrails correctly represent the expert's current procedure (not just syntactically valid) | Distinguishes prompts that were formally elicited from an SME vs. prompts that were written speculatively by a developer and never validated | ↑ Target: 100% for prompts that feed real decisions | Every production judgment prompt is traceable to an expert who confirmed its criteria are correct |

> **Note on deferred KPIs:** Metadata completeness rate, dependency map coverage, change governance compliance rate, and stale prompt ratio are important KPIs — but they measure generic data-asset governance quality that applies equally to any managed data asset. They are more appropriate as KPIs in the A-2 (Metadata) and C-3 (Lineage) topics. The five KPIs above are specific to prompts and harnesses as a content type.

> ▸ **Backup:** [Backup 18-1] KPI formulas and measurement method

#### [Backup 18-1] KPI formulas and measurement method

| KPI | Formula | Data source | Frequency |
|---|---|---|---|
| **Work Procedure Capture Rate** | (Key procedures with ≥1 registered prompt or harness asset / Total identified key procedures) × 100 | Procedure inventory (business process map) + prompt registry | Quarterly |
| **Prompt Anatomy Completeness Rate** | (Production-status prompts with all 7 anatomy fields non-empty / Total production-status prompts) × 100 | Registry query: production prompts where all anatomy fields are populated | Monthly (auto-calculated) |
| **Harness Reuse Rate** | (Executions of parameterized harnesses shared across ≥2 contexts / Total executions of that procedure type) × 100 | Runtime execution log + harness registry (count distinct deployment contexts per harness) | Monthly |
| **Judgment-Fidelity Re-validation Rate** | (Production prompts with dependency change in last 6 months AND SME re-validation completed / Total production prompts with dependency change in last 6 months) × 100 | Dependency change log + SME sign-off timestamp in registry | Monthly |
| **SME-Validated Procedure Coverage** | (Production prompts with SME sign-off field populated and dated within 2 years / Total production prompts feeding real decisions) × 100 | Registry `reviewed_by` + `sme_validated_at` fields | Quarterly |

---

## 7. Roadmap — Scale-up roadmap

### [Slide 19] Three-stage roadmap — from first capture to cross-affiliate harness library

**👉 One-line summary:** The roadmap progresses from capturing the most critical work procedures as structured prompts (short term), to standardizing anatomy templates and packaging as reusable harnesses (mid term), to a cross-affiliate shared harness library that makes expert knowledge a permanent organizational asset (long term).

**Headline:** Start with the highest-stakes judgment tasks — capture them properly, build the anatomy, validate with the SME. Then standardize the template so all teams capture the same way. Then share.

**Short term (months 1–6): Capture top work procedures as structured prompts**
*Goal: Convert the most critical tacit knowledge into managed, anatomy-complete prompt assets*

- Identify the 10–20 most critical work procedures relying on expert judgment (quality inspection calls, incoming material decisions, safety classifications) — these are the highest-knowledge-loss risk
- Train prompt engineers on the seven-component anatomy and the SME capture technique
- Conduct structured capture sessions with the domain experts who own these procedures
- Register the first wave of prompts with all seven anatomy fields complete and SME-validated
- Build golden example sets for each prompt (minimum 5 cases: standard PASS, standard REJECT, borderline, edge case, failure case)
- Baseline KPIs: work procedure capture rate (likely < 20%); anatomy completeness rate (likely < 50%)

**Mid term (months 7–18): Standardize anatomy template + package as reusable harnesses**
*Goal: Make the practice systematic and the prompts reusable across lines and teams*

- Roll out a **standard prompt anatomy template** (YAML schema) across all teams; require all new prompts to fill all seven components before registration
- Identify the most-used judgment workflows as candidates for harness packaging — those used across multiple lines, plants, or teams
- **Build parameterized harnesses** for the top 5–10 workflows: inspection judgment, incoming inspection, failure-mode classification, maintenance call, customer-complaint routing
- Validate harnesses end-to-end including fallback scenarios
- Establish the governance cadence: annual SME re-validation, triggered re-validation on standard changes
- Target KPIs: work procedure capture rate > 60%; anatomy completeness rate > 90% for production prompts; harness reuse rate > 30%

**Long term (month 19+): Shared cross-team/affiliate harness library**
*Goal: Make expert judgment a permanent, shared organizational asset — not locked in individual plants or teams*

- Build a **cross-affiliate shared prompt and harness library**: casting inspection harness from Plant A available (with governance) to Plants B, C, and D without re-elicitation
- Knowledge transfer protocol: when an expert retires or moves, the handover checklist includes reviewing all prompts they own, re-validating anatomy with their successor, and updating `owner` + `sme_validated_at`
- Automated anatomy completeness enforcement: prompts failing anatomy completeness check cannot advance from draft to staging
- Target KPIs: work procedure capture rate > 80% of identified key procedures; anatomy completeness 100% for production prompts; harness reuse rate 40–60%; SME-validated coverage 100% for decision-critical prompts

🏭 **Manufacturing perspective:** This roadmap mirrors how plants implement knowledge management: "What judgment does each expert hold that we would lose if they left?" → capture → standardize → share. The difference from paper SOPs: prompt assets carry machine-executable criteria and can be parameterized to run across all lines and plants with governance.

---

## Appendix

### [Appendix A] Full SME capture interview guide (extended)

Use alongside the capture checklist in [Backup 13-1]. This guide covers multi-session elicitation for complex judgment tasks.

**Pre-interview preparation:**
- Identify the SME whose expertise the prompt will encode
- Map out the task boundaries: what starts the judgment, what ends it, what are the inputs and outputs
- Prepare 3–5 real historical cases: 2–3 "standard" outcomes, 1–2 edge cases, 1 failure or near-miss case if available
- Book 60–90 minutes for Session 1 (initial elicitation); plan a 30-minute Session 2 (validation)
- Bring the organizational glossary: verify that domain terms used by the SME match the standard glossary

**Session 1 — Initial elicitation:**
Opening: "We are going to document your judgment process so an AI assistant can apply the same criteria consistently. Please walk through this task step by step as if explaining it to a new colleague who is skilled but unfamiliar with your specific criteria. Narrate what you look at, what you check, how you decide, and especially the exceptions and boundary conditions — those are the most important parts."

Full elicitation question bank:

| Area | Questions |
|---|---|
| **Procedure steps** | "Walk me through what you do, step by step, from start to finish." / "After you check X, what's next?" |
| **Judgment criteria** | "What are the exact numbers or conditions that determine your decision?" / "If you had to write down the rules, what would they say?" |
| **Reference standards** | "What documents, tables, or standards do you refer to?" / "Is there a document number for that?" |
| **Edge cases** | "When is it hardest to decide?" / "What's a case that looks like PASS but should be REJECT?" |
| **Guardrails** | "What would you NEVER do, no matter what the measurements show?" / "What's the one rule you'd never break?" |
| **Examples** | "Give me a clear PASS. Now a clear REJECT. Now a borderline case — and explain exactly why each one goes that way." |
| **Input data** | "What information do you need in front of you before you can start?" / "What would you be missing if the system only gave you X?" |
| **Output** | "What do you produce at the end — what do you write, record, or communicate?" |
| **Scope** | "Does this procedure apply the same way to all product lines / all plants?" / "Are there situations where you'd apply different criteria?" |

**Session 2 — Validation:**
Present the draft prompt to the SME. Give them 3 real cases and ask: "Would you evaluate these the same way this prompt would?" Walk through discrepancies. Revise criteria. Confirm that guardrails are stated correctly. Ask: "Is there anything this prompt might do that you would never do?"

---

### [Appendix B] Prompt asset anatomy registration checklist

Use this checklist before submitting a prompt to the registry. All "Required for registration" items must be complete; all "Required before production" items must be complete before `production` status.

**Anatomy completeness (D-3 specific — required for registration):**
- [ ] ① Role / persona defined in system message
- [ ] ② Task instruction: ordered steps of the work procedure
- [ ] ③ Context / grounding: reference standards and documents named explicitly
- [ ] ④ Judgment criteria: numbered, explicit thresholds and rules (NOT vague descriptions)
- [ ] ⑤ Output contract: typed schema with enums — NOT free text
- [ ] ⑥ Guardrails: at least one explicit forbidden action
- [ ] ⑦ Few-shot examples: at least 3 validated input→output pairs from historical records
- [ ] SME has confirmed that ④ and ⑥ correctly represent their procedure

**Metadata and infrastructure (generic data-asset discipline — required for production):**
- [ ] `name` (slug), `version`, `owner` (named individual), `status` (start as draft), `commit_message` populated
- [ ] All `input_variables` named with type and description
- [ ] `execution_settings` (model name + temperature) specified
- [ ] Dependency record: all data sources, standards, glossary terms listed
- [ ] `eval_datasets` reference to golden set in E-3
- [ ] `approved_for` scope defined (which lines/plants/teams)
- [ ] No hardcoded secrets, PII, or internal credentials in template

**Production promotion gates:**
- [ ] Anatomy completeness check: all seven components non-empty
- [ ] Golden eval: accuracy ≥ agreed threshold on E-3 golden set
- [ ] Guardrail test: adversarial inputs confirm forbidden actions are enforced
- [ ] SME sign-off: SME has reviewed the prompt with real test cases and confirmed criteria are correct
- [ ] Domain owner approval signed

---

### [Appendix C] Prompt/harness asset naming convention

**Prompt naming:** `{intent}.{domain}.{task-name}.v{version}`

| Segment | Values | Example |
|---|---|---|
| `intent` | classify / summarize / extract / generate / judge / draft / validate / plan | `judge` |
| `domain` | quality-control / finance / customer-service / logistics / maintenance / engineering | `quality-control` |
| `task-name` | hyphen-separated description of the specific task | `casting-defect-severity` |
| `v{version}` | Integer (registry) or semantic version (MAJOR.MINOR.PATCH) | `v2` or `v2.1.0` |

**Example full names:**
- `judge.quality-control.casting-defect-severity.v2`
- `draft.customer-service.warranty-claim-response.v3`
- `classify.maintenance.failure-mode-rootcause.v1`

**Harness naming:** `{domain}.{workflow-name}-harness.v{version}`
- `quality-control.casting-defect-triage-harness.v1`
- `customer-service.complaint-response-drafting-harness.v2`
- `logistics.incoming-inspection-judgment-harness.v2`

**Tag taxonomy (all prompts must have at least one tag per facet):**
- **Intent facet:** classify | summarize | extract | generate | judge | draft | validate | plan
- **Domain facet:** quality-control | finance | customer-service | logistics | maintenance | engineering | hr | legal
- **Modality facet:** text-to-text | text-to-json | document-to-text | multi-turn | image-to-text

---

## References

- [Prompt Engineering Is Dead – Long Live PromptOps (Dataversity)](https://www.dataversity.net/articles/prompt-engineering-is-dead-long-live-promptops/) — src-050
- [Why Large Enterprises Need a Prompt Registry for AI Governance (Martin Rodek)](https://medium.com/@martin_rodek/why-large-enterprises-need-a-prompt-registry-for-ai-governance-04d744039bb4) — src-051
- [Managing Prompts as Enterprise Assets: A Portfolio Approach (Segev Shmueli / Medium)](https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327) — src-052, src-107, src-154, src-200
- [Managing Prompt Technical Debt in Enterprise AI (Wipro)](https://wiprotechblogs.medium.com/managing-prompt-technical-debt-in-enterprise-ai-7985f4bc7ff7) — src-053
- [Prompt Governance: Enabling Reusability and Standards in AI-Driven Organizations (Bojan Ciric / Deloitte Fellow)](https://medium.com/the-future-of-data/prompt-governance-enabling-reusability-and-standards-in-ai-driven-organizations-1d4dfe0a83ea) — src-054
- [Tacit Knowledge in Manufacturing: Capture It Before It's Gone (Dovient)](https://dovient.com/resources/blog/tacit-knowledge-manufacturing-hidden-asset) — src-055
- [Prompt Versioning: The Complete Guide (Agenta)](https://agenta.ai/blog/prompt-versioning-guide) — src-057
- [Prompt Versioning Best Practices for AI Engineering Teams (Maxim AI)](https://www.getmaxim.ai/articles/prompt-versioning-best-practices-for-ai-engineering-teams/) — src-058, src-156
- [Prompt Governance Is the New Data Governance (CIO Magazine)](https://www.cio.com/article/4130960/prompt-governance-is-the-new-data-governance.html) — src-059
- [What Is Prompt Drift? Definition & Examples (PE Collective)](https://pecollective.com/glossary/prompt-drift/) — src-060
- [How to Build AI Audit Trails That Stand Up to Regulatory Scrutiny (CX Today)](https://www.cxtoday.com/security-privacy-compliance/ai-audit-trail-regulatory-scrutiny/) — src-061
- [Tacit Knowledge Elicitation Process for Industry 4.0 (Springer / Discover AI)](https://link.springer.com/article/10.1007/s44163-022-00020-w) — src-062
- [Prompt Registry for LLMs & Agents (MLflow)](https://mlflow.org/prompt-registry) — src-063
- [Concepts – Langfuse Prompt Management](https://langfuse.com/docs/prompt-management/data-model) — src-100
- [What is Prompt Management? (Braintrust)](https://www.braintrust.dev/articles/what-is-prompt-management) — src-101
- [Prompt Registry Overview – PromptLayer](https://docs.promptlayer.com/features/prompt-registry/overview) — src-104
- [Prompt Registry – MLflow AI Platform](https://mlflow.org/docs/latest/genai/prompt-registry/) — src-105
- [The Prompt Lifecycle – NeoSage / Dimple Sharma](https://blog.neosage.io/p/the-prompt-lifecycle-every-ai-engineer) — src-106
- [Tacit Knowledge in Manufacturing – Augmentir](https://www.augmentir.com/glossary/tacit-knowledge) — src-108
- [What is Prompt Management? – LangWatch](https://langwatch.ai/blog/what-is-prompt-management-and-how-to-version-control-deploy-prompts-in-productions) — src-109
- [Amazon Bedrock Prompt Management (AWS)](https://aws.amazon.com/bedrock/prompt-management/) — src-110
- [BAML – BoundaryML (GitHub)](https://github.com/BoundaryML/baml) — src-111
- [Structured-Prompt-Driven Development (SPDD) (Wei Zhang, Jessie Xia / martinfowler.com)](https://martinfowler.com/articles/structured-prompt-driven/) — src-152
- [Prompt, Agent, and Model Lifecycle Management (AWS Prescriptive Guidance)](https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-serverless/prompt-agent-and-model.html) — src-153
- [Prompt Versioning & Management Guide (LaunchDarkly Blog)](https://launchdarkly.com/blog/prompt-versioning-and-management/) — src-155
- [Reusable Prompt Libraries: Taxonomy and Access (PulseGeek / Evie Rao)](https://pulsegeek.com/articles/how-to-create-reusable-prompt-libraries-taxonomy-metadata-and-access/) — src-157
- [BAML – The AI Framework (BoundaryML)](https://github.com/BoundaryML/baml) — src-158
- [Prompt Governance: The Emerging Enterprise Control Layer (Fulcrum Digital)](https://fulcrumdigital.com/blogs/prompt-governance-the-emerging-enterprise-control-layer/) — src-159
- [Manage Prompts – LangSmith Docs](https://docs.langchain.com/langsmith/manage-prompts) — src-160
- [Agenta – Open-source LLMOps Platform](https://agenta.ai/) — src-161
- [What is Prompt Versioning? (Braintrust)](https://www.braintrust.dev/articles/what-is-prompt-versioning) — src-162
- [Agent Harness Engineering (Addy Osmani)](https://addyosmani.com/blog/agent-harness-engineering/) — src-163
- [Advanced LLM Security: Preventing Secret Leakage (Doppler)](https://www.doppler.com/blog/advanced-llm-security) — src-165
- [Tacit Know-How Made Visible: Using Conversational AI for Business-Process Documentation (arXiv, Dec 2025)](https://arxiv.org/pdf/2512.05122) — src-166
- [Prompt Editor: A Taxonomy-driven System for Guided LLM Prompt Development in Enterprise Settings (ACM SIGMOD 2025)](https://dl.acm.org/doi/10.1145/3722212.3725124) — src-167
- [Prompt Library Metrics: Coverage, Regression Framework 2026 (Digital Applied)](https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026) — src-205
