# D-3 Prompt/Harness Asset-ization — KPI / Roadmap Research Notes

**Topic**: D-3 — KPI and Maturity/Roadmap cluster  
**Researcher**: Subagent (claude-sonnet-4-6)  
**Date**: 2026-06-10  
**Weight**: ~15% of D-3 total

---

## 1. Why KPIs Matter for Prompt/Harness Assets — The Plain Cause-Effect

Prompts and harnesses are the "invisible configuration" of AI systems. Unlike application code, they are frequently edited informally (in notebooks, inline strings, ad-hoc comments) with no versioning, no test gates, and no ownership record. This makes them structurally similar to **untracked SOPs in manufacturing**: a line worker edits a procedure without updating the official document; quality drift is invisible until a defect batch ships.

The direct cause-effect chain:

| Absence | Effect | Lagging signal |
|---------|--------|----------------|
| No registry coverage | Unknown prompts live in production; no audit trail | Incidents post-mortem says "someone changed the prompt last week, I think" |
| No eval coverage | No automated quality gate; changes go live untested | Support ticket volume spikes |
| No regression cron | Model-vendor updates and dataset drift cause silent quality drops | Revenue-impacting failures discovered by users, not engineers |
| No lifecycle policy | "Zombie" prompts accumulate; deprecated prompts continue receiving traffic | Escalating maintenance cost; security exposure from unmaintained prompt logic |
| No owner assignment | Shared ownership = no ownership; alerts go to team channel, nobody answers | Regressions compound for days before anyone acts |

The manufacturing analogy that works best: **SOP (Standard Operating Procedure) compliance rate** in ISO-certified factories. The metric measures what percentage of procedures have current, signed-off documentation with a named owner. Prompt registry coverage is the same KPI for AI operational procedures. [src-404]

A widely cited engineering postmortem (cited in Deepchecks, src-404): "Three words added to improve 'conversational flow' caused structured-output error rates to spike dramatically within hours, halting revenue-generating workflows until engineers manually rolled back the change." Without version history, root-cause analysis is guesswork.

---

## 2. KPI Candidates — Authoritative Sources

### 2.1 The 10-KPI Panel (Digital Applied, 2026)

The most comprehensive single-source KPI framework for prompt operations comes from Digital Applied's "Prompt Library Metrics" framework [src-401], drawing from Promptfoo, OpenAI Evals, and DeepEval tooling. It organizes 10 KPIs across 6 axes:

#### Axis A — Catalog Coverage (3 KPIs)

| KPI | Definition | Formula | Target | Plain explanation |
|-----|-----------|---------|--------|-------------------|
| Catalog completeness | % of production prompts with full metadata row | (Prompts with owner + model + version + edit date + lifecycle stage + feature) / All production prompts | >95% | Like measuring % of SOPs with a named author and last-review date. Typical starting baseline: 40–60%. |
| Owner uniqueness | % of prompts with exactly one named human owner | (Prompts with exactly 1 named owner) / All catalog entries | 100% | Shared ownership = no ownership. If nobody is accountable, nobody responds to alerts. |
| Catalog freshness | Days since last end-to-end metadata audit | Days elapsed since last catalog walk | <30 days | Like calibration expiry in a lab: if the instrument hasn't been checked recently, you can't trust its readings. |

#### Axis B — Eval Coverage (threshold-based maturity)

| KPI | Definition | Threshold | Plain explanation |
|-----|-----------|-----------|-------------------|
| Eval coverage | % of production prompts wired into at least one passing eval suite | <50%: library is a "wish"; >80%: library is a measurable system; >95%: engineering parity | Like % of SOPs with a documented acceptance test. Below 50%, regressions are detected via support tickets. Above 80%, model migrations take 15 minutes instead of a panic week. |

Four coverage tiers [src-401]:
- **Stage 1 (ad-hoc)**: <50% — most prompts unverified
- **Stage 2 (catching up)**: 50–80% — high-stakes prompts covered, long tail blind
- **Stage 3 (prompt-ops)**: >80% — production behavior quantified
- **Stage 4 (institutional)**: >95% — new prompts cannot ship without eval suites

#### Axis C — Regression Rate (3 KPIs)

| KPI | Definition | Formula | Target |
|-----|-----------|---------|--------|
| Daily regression rate | Share of evaluated prompts whose nightly eval score dropped beyond tolerance | (Prompts with score drop > tolerance) / Evaluated prompts | ≤2% daily; >5% = investigation trigger |
| Time-to-acknowledge | Median time from regression alert to named owner acknowledging | Median(alert_time → first_owner_action) | <24 h (business days); >72 h = broken routing |
| Score history retained | Days of nightly eval scores stored | Days of rolling history | 30+ days |

**Why regression rate is the leading indicator**: Teams that track it catch vendor-model updates 1–7 days before they become customer-visible quality drops. [src-404] The daily cron is the single highest-leverage infrastructure investment in the entire KPI stack. [src-407]

Analogy: SPC (Statistical Process Control) charts in manufacturing — daily sampling of output quality to catch drift before defects reach customers.

#### Axis D — Model-Fit Divergence (2 KPIs)

| KPI | Definition | Target | Practical finding |
|-----|-----------|--------|-------------------|
| Cross-tier coverage | % of production prompts with recent eval on a cheaper-tier model | >70% | Identifies the ~1/3 of library that can migrate to lower-cost model with no measurable quality loss |
| Median divergence | Median eval-score gap between primary and cheaper-tier model | Track trend | Prompts with gap ≤ tolerance are migration-ready; cost reduction 30–50% for that subset [src-401] |

**CFO language**: "Model-fit divergence is the metric that turns the prompt panel into a budget conversation." The cost lever is identifying which prompts genuinely require expensive models versus which ones are over-provisioned.

#### Axis E — Lifecycle Adherence (2 KPIs)

| KPI | Definition | Formula | Target |
|-----|-----------|---------|--------|
| Stage accuracy | % of prompts in the correct lifecycle stage | (Prompts with stage matching traffic state) / All catalog entries | >95% |
| Deprecation latency | Median days from deprecation to retirement (removal from codebase) | Median(deprecation_date → retirement_date) | <60 days |

Lifecycle stages: **draft → production → deprecated → retired** [src-401, src-407]

Without a lifecycle KPI, deprecated prompts linger for quarters receiving traffic and accumulating eval failures that nobody investigates.

#### Axis F — Dashboard Cadence (governance KPI)

Cadence itself is a KPI: "A KPI without a cadence is a screensaver." [src-401]

| Loop | Frequency | Content |
|------|-----------|---------|
| Automation | Nightly | Eval suites run, regression detection fires, owner-routed alerts |
| Awareness | Weekly | 5-minute standup: regression count, eval coverage delta, stale owner acknowledgements |
| Hygiene | Monthly | Catalog walk: stage accuracy, deprecation schedule, migration docs |
| Stakeholder | Quarterly | Panel to product leadership + finance: coverage trajectory, cost savings, lifecycle hygiene |

### 2.2 Additional KPIs from LLMOps Practice

Beyond the 10-KPI panel, LLMOps practitioners cite:

**Change lead time** — Time from prompt change authoring to validated production deployment. Analogy to DORA metrics for software delivery. With a registry + CI pipeline, this compresses from days (ad-hoc deploy cycles) to hours or minutes. [src-403, agenta.ai CI/CD reference]

**Prompt reuse rate** — % of new task implementations that reuse an existing harness or prompt from the registry rather than authoring from scratch. Assembled's case (src-406): modularizing prompts for specific tasks → "consulting the library before crafting from scratch." Higher reuse rate = faster time-to-onboard a new task.

**Time-to-onboard a new task using existing harnesses** — Operational measure of harness reuse value. Directly reduced when the library has a searchable catalog, tag-based discovery, and well-documented prompts. [src-403 — MLflow search_prompts API by tag; src-406]

**User-edit/override rate** — % of AI outputs that a human user subsequently edits or overrides. High override rate signals the prompt/harness does not match user judgment criteria — a signal for prompt refinement. Cited indirectly by Ubisoft (src-406) where human scriptwriters edit AI-generated game dialogue; the edit rate is a proxy for prompt quality.

**Audit-trail completeness** — % of production prompt deployments with a full audit record (who changed, when, why, what eval scores accompanied the change). MLflow provides this via immutable version history + commit messages. Cited as compliance requirement for regulated industries. [src-403]

---

## 3. Maturity Staging — How Organizations Phase This In

Four major frameworks converge on a common progression. The table below synthesizes Microsoft Azure [src-402], IBM [src-408], and ZenML's comparative analysis [src-405]:

### 3.1 Synthesized 5-Stage Maturity Model for Prompt/Harness Asset-ization

| Stage | Label | What exists | Prompt/Harness state | Key KPIs operational |
|-------|-------|-------------|---------------------|----------------------|
| 0 | Ad-hoc | No registry; prompts hardcoded inline or scattered in notebooks | Tribal knowledge; "someone wrote this and they left" | None |
| 1 | Inventoried | Catalog audit done; all prompts in one directory; owners assigned; naming convention | Prompts are findable; catalog coverage >80% | Catalog completeness; owner uniqueness |
| 2 | Versioned Registry | Immutable version history; commit messages; aliases (dev/staging/prod); basic eval suites for high-stakes prompts | Prompts are reproducible and debuggable | Eval coverage >50%; change lead time tracked |
| 3 | Governed with CI/regression | PR-gated eval before deploy; nightly regression cron; model-fit testing; lifecycle policy written; governance forum meets monthly | Prompts are engineered assets: testable, deployable, rollback-capable | Daily regression rate ≤2%; eval coverage >80%; deprecation latency <60d; audit-trail completeness |
| 4 | Harness reuse at scale | Reusable harness packages; dependency mapping; tag-based discovery; harness composition for new tasks; integration with eval (E-3) and feedback loop (E-4) | Prompts/harnesses are institutional capital: new tasks built from existing components | Prompt reuse rate; time-to-onboard new task; model-fit divergence <threshold for budget decisions |

### 3.2 Microsoft Azure's 4-Level Model (direct source) [src-402]

| Level | Name | Prompt governance |
|-------|------|-------------------|
| 1 | Initial | Basic prompt experiments; manual; no versioning |
| 2 | Defined | Structured prompt design; meta prompt templates; RAG integration; basic eval metrics |
| 3 | Managed | Version control with rollback; CI/CD for LLM apps; batch evaluation; real-time monitoring |
| 4 | Optimized | Auto-approval CI/CD; predictive monitoring; A/B testing; automated prompt refinement |

### 3.3 IBM's 5-Phase Model — Governance Column [src-408]

| Phase | Governance Maturity | Prompt Capability Added |
|-------|---------------------|------------------------|
| 1 | None | Manual prompt engineering |
| 2 | Some policies | Prompt Engineering as a discipline begins |
| 3 | Common metrics to govern AI lifecycle | Systematic prompt engineering; access policy management |
| 4 | Automated validation and monitoring | Bias/HAP detection; compliance adherence; quantitative eval |
| 5 | Fully automated AI lifecycle governance | Prompt Monitoring and Security; advanced fine-tuning |

IBM Phase 3 = "common set of metrics to govern AI lifecycle" is the inflection point where registry coverage, eval coverage, and regression rate become operational instruments rather than nice-to-haves.

---

## 4. Roadmap — Phased Implementation Practices

### 4.1 The 30/60/90-Day Rollout Roadmap [src-407]

The most detailed practitioner roadmap available is Digital Applied's 90-day plan (2026), validated across teams from 2-person AI startups to 30-person product organizations.

**Days 1–30: Foundation (Catalog + Versioning)**
- Catalog audit: grep codebase for all prompt files; build inventory CSV
- Naming + versioning convention: kebab-case slug + monotonic version (CONVENTIONS.md)
- Migrate to single source: prompts/ directory; no inline strings in service handlers
- Owner assignment: CODEOWNERS enforced; exactly one human per prompt
- README + team demo

*Key metric at day 30*: catalog completeness baseline established (typical start: 40–60%)

**Days 31–60: Feedback Loop (Evals + Regression)**
- Choose eval framework (Promptfoo for TypeScript, DeepEval for Python AI teams, OpenAI Evals for OpenAI-anchored stacks)
- First eval suite: 10 test cases on highest-stakes prompt (happy path, edge cases, known failures)
- CI integration: PR-gated eval; block merge on regression below rubric threshold
- Regression cron: nightly full-library run; 30-day score history; owner-routed alerts (NOT shared channel)
- Model-fit testing: top 5 prompts vs. cheaper-tier alternative

*Key metrics at day 60*: eval coverage %, daily regression rate, model-fit divergence per prompt

**Days 61–90: Governance + Lifecycle + Social Layer**
- Governance gates: new-prompt template (must include eval suite, owner, model-fit decision, lifecycle target stage)
- Lifecycle policy: LIFECYCLE.md defining beta/production/deprecated/archived transitions + deletion dates
- Team training: 1-hour workshop + reference deck → becomes onboarding material
- Standing monthly governance forum: eval coverage trends, deprecation queue, lifecycle decisions
- Handoff: rollout team steps back; 1 full week running independently before declaring complete

*Key metrics at day 90*: deprecation latency, stage accuracy, governance cadence adherence

### 4.2 Starting Points — High-Value Repeated Tasks First

Consistent with "start with business value" guidance from ZenML [src-405] and all maturity models:

1. **Identify the 5 highest-stakes repeated tasks** — these are where prompt drift causes the most business damage. Start the registry pilot here.
2. **Pilot registry with a single team** — prove the cycle (catalog → version → eval → cron → alert) before expanding to the whole organization.
3. **Expand catalog coverage** — enforce new-prompt template via governance gate; coverage climbs from 40–60% to 90%+ within two weeks of committing to the metric [src-401].
4. **Add dependency mapping and harness packaging** — once individual prompts are governed, package related prompts + instructions into reusable harness units for cross-task reuse.
5. **Integrate with eval (E-3) and feedback loop (E-4)** — the regression cron and model-fit testing connect to: (a) E-3 (eval datasets) as the ground truth for nightly runs; (b) E-4 (feedback loop) to close the improvement cycle using production signal.

### 4.3 The Critical Failure Mode to Avoid

The #1 cause of rollout failure is the **eval-before-catalog inversion**: writing eval suites before naming/versioning is finalized. When prompts get renamed during the single-source migration (days 13–20), all previously written evals must be rewritten. Enforce stage sequencing. [src-407]

The #2 failure is **shared-channel alert routing**: regression alerts sent to a team Slack channel go unread. Every alert must be routed to the named owner from the catalog frontmatter. The owner field and alert routing target must be the same field. [src-401, src-407]

---

## 5. Backup Slide Detail — Metric Formulas

For deep-dive slides:

### Registry Coverage Formula
```
Registry Coverage = (Production prompts in registry with complete metadata) / (All production prompts in codebase) × 100
```
Numerator requires: slug, version, owner, model, status, feature, eval-suite path, last-edited date.  
Start from a codebase grep; do not self-report from the registry alone (the registry doesn't know what it doesn't know).

### Eval Coverage Formula
```
Eval Coverage = (Production prompts with ≥1 passing eval suite wired to CI and nightly cron) / (All production prompts) × 100
```
Binary at suite level: partial coverage (e.g., 3 of 10 test cases) counts as 0 to prevent gaming.

### Daily Regression Rate Formula
```
Daily Regression Rate = (Prompts with nightly eval score drop > tolerance) / (Prompts evaluated last night) × 100
```
Tolerance: typically ±2 percentage points on a 100-point rubric or ±1 point on a 5-point scale.  
Sustained >2% over 7 days = structural problem.  
Single-night spike >5% = investigation trigger.

### Model-Fit Divergence Formula
```
Divergence(prompt_i) = EvalScore(prompt_i, primary_model) - EvalScore(prompt_i, cheaper_tier_model)
```
Migration-ready threshold: divergence ≤ tolerance (usually 2–3 points on a 100-point scale).  
Typical library distribution: ~1/3 migration-ready, ~1/3 migration-blocked, ~1/3 middle (may migrate with light edits).

### Change Lead Time Formula
```
Change Lead Time = deployment_timestamp - authoring_timestamp (of same prompt version)
```
Baseline (no registry, inline strings): days to weeks (require full app redeploy).  
With registry + CI pipeline: minutes to hours.

---

## 6. Integration with Other D-3 Clusters

KPIs and Roadmap connect to other D-3 clusters as follows:

| Connection | Detail |
|-----------|--------|
| **D-3 What/How** | The registry, versioning, and lifecycle concepts are the "what"; the eval integration is the "how". KPIs measure whether the "what" is being maintained. |
| **E-3 (Eval Data)** | Nightly regression cron consumes eval datasets from E-3. Eval coverage KPI is empty without E-3's ground-truth test cases. |
| **E-4 (Feedback Loop)** | User-edit/override rate (E-4 signal) feeds back into prompt refinement. Model-fit divergence tracking creates a cost optimization loop. |
| **D-2 (Tool Specs)** | KPIs here cover instruction/judgment logic ONLY. Tool specifications are out of scope. |

---

## Sources

| src id | URL | Title |
|--------|-----|-------|
| src-401 | https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026 | Prompt Library Metrics: Coverage, Regression Framework 2026 |
| src-402 | https://azure.microsoft.com/en-us/blog/achieve-generative-ai-operational-excellence-with-the-llmops-maturity-model/ | Achieve generative AI operational excellence with the LLMOps maturity model (Microsoft Azure) |
| src-403 | https://mlflow.org/prompt-registry  /  https://mlflow.org/docs/latest/genai/prompt-registry/ | Prompt Registry for LLMs & Agents — MLflow |
| src-404 | https://deepchecks.com/llm-production-challenges-prompt-update-incidents/ | Solving LLM Production Challenges: How Prompt Updates Drive Most Incidents (Deepchecks) |
| src-405 | https://www.zenml.io/blog/everything-you-ever-wanted-to-know-about-llmops-maturity-models | Everything you ever wanted to know about LLMOps Maturity Models (ZenML) |
| src-406 | https://www.zenml.io/blog/prompt-engineering-management-in-production-practical-lessons-from-the-llmops-database | Prompt Engineering & Management in Production: Practical Lessons from the LLMOps Database (ZenML) |
| src-407 | https://www.digitalapplied.com/blog/prompt-library-team-rollout-30-60-90-day-plan-2026 | Prompt Library Team Rollout: 30/60/90-Day Plan 2026 (Digital Applied) |
| src-408 | https://www.ibm.com/think/architectures/patterns/genai-adoption-maturity-model | The IBM Maturity Model for GenAI Adoption: A 5-Phase Framework (IBM) |
| src-409 | https://docs.datadoghq.com/llm_observability/monitoring/prompt_tracking/ | Prompt Tracking — Datadog Agent Observability Documentation |
| src-410 | https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity?view=azureml-api-2 | Advance your maturity level for GenAIOps — Microsoft Learn (Azure Machine Learning) |

---

## 7. Additional Verified Material (2026-06-10 supplement)

### 7.1 Microsoft Learn GenAIOps Assessment — Numeric Score Bands [src-410]

Microsoft's official self-assessment (https://learn.microsoft.com/en-us/assessments/e14e1e9f-d339-4d7e-b2bb-24f056cf08b6/) maps questionnaire scores to the 4 maturity levels:

| Level | Score | Label | Prompt/Harness Asset-ization state |
|-------|-------|-------|-------------------------------------|
| 1 | 0–9 | Initial | No structured prompt practices; pure experimentation |
| 2 | 10–14 | Defined | Structured prompt design begins; basic CI/CD integration explored |
| 3 | 15–19 | Managed | "Strengthen asset management strategies through advanced version control and rollback" — explicit Level 3 capability |
| 4 | 20–28 | Optimized | Predictive analytics, comprehensive content safety, continuous refinement |

**Key use for writers**: The score range (0–28) gives organizations a self-benchmarking tool. An organization scoring 10–14 that wants to reach Level 3 needs, per Microsoft's own guidance, to implement "advanced version control and rollback capabilities" for prompt assets. This is the official Microsoft endorsement of the D-3 capability at a specific maturity stage.

Page last updated: 2025-11-18 (stable official documentation).

### 7.2 Datadog Prompt Tracking — Production KPI Tooling [src-409]

Datadog's Agent Observability Prompt Tracking feature (generally available) is a primary-source confirmation that the following KPIs are operationally achievable in enterprise-scale tooling:

| KPI | Datadog implementation |
|-----|------------------------|
| Audit-trail completeness | Every LLM call automatically linked to prompt@name@version; stored in traces with full telemetry |
| Regression detection | Latency, token count, cost, and eval metrics per version — visualized as timeseries; side-panel diff view between versions |
| Incident investigation | Filter Trace Explorer by prompt name, ID, or version → isolate impacted requests from any historic timeframe |
| Prompt change lead time | Time-of-last-update visible in "Recent Prompt Updates" panel; call count before/after version change visible |
| Cost tracking | "Most Tokens Used" and "Highest Latency Prompts" rankings → direct cost attribution per prompt |

**Prompt metadata schema** (fields that map to KPI axes):
- `name`, `id`: registry catalog identity
- `version`: lifecycle stage tracking
- `template` / `chat_template`: immutable content hash
- `variables`: runtime parameter tracking
- `rag_context_variables`, `rag_query_variables`: eval ground-truth linkage

**Auto-instrumentation**: LangChain prompt templates captured automatically without code changes — confirms standard tooling support without custom engineering.

**Key implication**: Audit-trail completeness (KPI) is not a theoretical aspiration; it is a standard Datadog product feature. Organizations not using this capability are leaving an operational control gap.
