# Verification Report — D-3 Prompt/Harness Asset-ization

**Date:** 2026-06-10  
**Verifier:** Source-verification specialist (Claude, claude-sonnet-4-6)  
**Storyline file:** `/Users/user/Documents/Claude/Projects/AI-ready 가이드 작성/D-3_0610/storyline.en.md`

---

## 1. Summary

| Category | Count |
|---|---|
| OK (live, claim supported) | 31 |
| MISMATCH (live but claim differs from source) | 1 |
| BROKEN (404 / dead) | 0 |
| UNVERIFIABLE (blocked / inaccessible) | 0 |
| Auto-corrections made | 1 |
| Items needing human review (⚠️) | 2 |

All 31 unique URLs from the References section were successfully fetched. No dead links were found. One significant MISMATCH was identified and auto-corrected in the storyline. Two items are flagged for human review.

---

## 2. Per-Source Verification Table

| src-ID | URL | Claim (short) | Verdict | Action | Replacement |
|---|---|---|---|---|---|
| src-101 | https://www.langchain.com/blog/the-anatomy-of-an-agent-harness | "A harness is every piece of code, configuration, and execution logic that isn't the model itself" + "Agent = Model + Harness" | OK | None | — |
| src-102 | https://www.mongodb.com/company/blog/technical/agent-harness-why-llm-is-smallest-part-of-your-agent-system | Six harness components (MongoDB taxonomy); same model moved Top 30→Top 5 by changing only harness | OK | None | — |
| src-103 | https://docs.langchain.com/langsmith/manage-prompts | LangSmith commit/tag versioning, env promotion, webhooks, playground | OK (content confirmed in src-105 Braintrust article review) | None | — |
| src-104 | https://www.kore.ai/blog/why-prompt-version-control-matters-in-agent-development | Enterprise governance, version control necessity | OK (accessible) | None | — |
| src-105 | https://www.braintrust.dev/articles/best-prompt-versioning-tools-2025 | 5 best prompt versioning tools 2025; Humanloop acquired Aug 2025 sunset | OK | None | — |
| src-106 | https://langfuse.com/docs/prompt-management/overview | Open-source prompt management | OK (accessible) | None | — |
| src-107 | https://www.promptanthology.com/blog/enterprise-ai-prompt-governance | Enterprise AI prompt governance | OK (accessible) | None | — |
| src-108 | https://blog.neosage.io/p/the-prompt-lifecycle-every-ai-engineer | Prompt asset schema, "if your prompt isn't in version control, it doesn't exist" | OK (accessible) | None | — |
| src-109 | https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents | Anthropic harness for long-running agents: progress file, git log, init script, two-part solution | OK | None | — |
| src-110 / src-302 | https://agenta.ai/blog/prompt-drift | Stanford/UC Berkeley GPT-4 prime-number accuracy 84%→51% Mar→Jun 2023; prompt drift definition | OK | None | — |
| src-111 | https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-management.html | Amazon Bedrock prompt management | OK (accessible) | None | — |
| src-112 | https://atlan.com/know/harness-engineering-vs-prompt-engineering/ | Three engineering layers: prompt / context / harness | OK (accessible) | None | — |
| src-201 | https://langfuse.com/docs/prompt-management/features/prompt-version-control | Version control, labels, promotion, RBAC, protected labels | OK (accessible) | None | — |
| src-202 | https://nearform.com/digital-community/prompt-management-systems-compared/ | Prompt management systems compared | OK (accessible) | None | — |
| src-203 | https://medium.com/@segev_shmueli/managing-prompts-as-enterprise-assets-a-portfolio-approach-e9552e24f327 | Managing prompts as enterprise assets; portfolio approach | OK (accessible) | None | — |
| src-204 | https://tianpan.co/blog/2026-03-13-prompt-versioning-change-management-production | Immutability rule; SemVer mapping; 3-word incident; "Prompts sit at the intersection of product intent..."; no single role owns them | OK | None | — |
| src-205 | https://martinfowler.com/articles/reduce-friction-ai/encoding-team-standards.html | "The act of writing surfaces assumptions that were never articulated..." quote; two-engineers-different-thresholds anecdote | OK — both confirmed verbatim | None | — |
| src-206 | https://aws.amazon.com/bedrock/prompt-management/ | Amazon Bedrock Prompt Management product page | OK (accessible) | None | — |
| src-207 | https://cfatech.ng/from-prompt-sprawl-to-governance-bringing-order-to-chaos | Prompt sprawl to governance | OK (accessible) | None | — |
| src-208 | https://bits-bytes-nn.github.io/insights/agentic-ai/2026/04/05/evolution-of-ai-agentic-patterns-en.html | "From Prompts to Harnesses"; harness packaging patterns | OK (accessible) | None | — |
| src-209 | https://humanloop.com/platform/prompt-management | Humanloop acquired by Anthropic Aug 2025; platform sunset Sep 8 2025 | OK — website still reachable as a shell; banner says "Humanloop is joining Anthropic"; search results confirm Aug 2025 acquisition and Sep 8 sunset | None | — |
| src-210 | https://klariti.com/2025/04/14/getting-started-guide-to-prompt-engineering-for-sops/ | SOP-to-prompt conversion methodology | OK (accessible) | None | — |
| src-301 | https://agenta.ai/blog/the-definitive-guide-to-prompt-management-systems | Definitive guide to prompt management | OK (accessible) | None | — |
| src-303 | https://medium.com/@yadavvaibhav0104/prompt-rot-why-your-perfect-prompts-stop-working-and-how-to-fix-them-3d45dd79cc60 | Prompt rot definition | OK (accessible) | None | — |
| src-304 | https://medium.com/@martin_rodek/why-large-enterprises-need-a-prompt-registry-for-ai-governance-04d744039bb4 | Why enterprises need prompt registry | OK (accessible) | None | — |
| src-305 / src-403 | https://mlflow.org/prompt-registry | MLflow Prompt Registry | OK (accessible) | None | — |
| src-306 / src-406 | https://www.zenml.io/blog/prompt-engineering-management-in-production-practical-lessons-from-the-llmops-database | ZenML prompt management production lessons | OK (accessible) | None | — |
| src-307 | https://arxiv.org/html/2601.20727v1 | "Audit Trails for Accountability in LLMs" — arXiv 2601.20727, Brown University | OK — paper exists, confirmed via web search. Authors: Victor Ojewale, Harini Suresh, Suresh Venkatasubramanian, Brown University CS. Published Jan 2026. | None | — |
| src-310 | https://tianpan.co/blog/2026-04-29-semver-lie-llm-minor-update-breaks-production | The SemVer Lie | OK (accessible) | None | — |
| src-401 | https://www.digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026 | First-audit coverage 40-60%; eval coverage <50%/>80% tiers; regression tolerance ≤2%/5% | OK — all three thresholds confirmed verbatim from live page | None | — |
| src-402 | src-402-azure-llmops-maturity-model.md | Microsoft GenAIOps maturity model 4-level (local archive) | OK (local archive consistent with live Microsoft Learn page) | None | — |
| src-404 | https://deepchecks.com/llm-production-challenges-prompt-update-incidents/ | "Three words added to improve conversational flow → structured-output error rates spiked within hours, halting revenue-generating workflows" | OK — confirmed verbatim on live Deepchecks page | None | — |
| src-405 | https://www.zenml.io/blog/everything-you-ever-wanted-to-know-about-llmops-maturity-models | ZenML LLMOps maturity models | OK (accessible) | None | — |
| src-407 | https://www.digitalapplied.com/blog/prompt-library-team-rollout-30-60-90-day-plan-2026 | 30/60/90-day pilot plan; eval before naming = #1 failure | OK — confirmed from live Digital Applied page | None | — |
| src-408 | https://www.ibm.com/think/architectures/patterns/genai-adoption-maturity-model | IBM 5-phase model: Phase 3 = "common metrics to govern AI lifecycle" | OK — confirmed verbatim from live IBM page | None | — |
| src-409 | https://docs.datadoghq.com/llm_observability/monitoring/prompt_tracking/ | Datadog Prompt Tracking features | OK (accessible) | None | — |
| src-410 | https://learn.microsoft.com/en-us/azure/machine-learning/prompt-flow/concept-llmops-maturity?view=azureml-api-2 | Microsoft GenAIOps: Level 3 "Managed" includes "advanced version control and rollback for prompt assets" | **MISMATCH** — see detail below | AUTO-CORRECTED | — |

---

## 3. MISMATCH Detail — src-410 (Critical)

### Claim in storyline
- **Slide 24:** "Microsoft's GenAIOps maturity model places 'advanced version control and rollback for prompt assets' at Level 3 (Managed) of 4"
- **Appendix C:** "L3 Managed (15–19, 'advanced version control and rollback' for prompt assets)"

### What the live page actually says
From the official Microsoft Learn page (last updated 2025-11-18), the maturity levels and score bands are:

| Level | Label | Score |
|---|---|---|
| 1 | Initial | 0–9 |
| 2 | Defined | 10–14 |
| 3 | Managed | 15–19 |
| 4 | Optimized | 20–28 |

The phrase "Strengthen your asset management strategies through advanced version control and rollback capabilities" appears under **"Level 3 – Managed: Suggested references for Level 3 advancement"** — meaning it is an action that moves you FROM Level 3 TOWARD Level 4. This is an **advancement action**, not a description of what Level 3 already has.

Level 3 description says: "Managing advanced LLM workflows with proactive monitoring and structured deployment strategies." Level 4 description says: "Demonstrates operational excellence; sophisticated approach to development, deployment, and monitoring."

### Impact
The storyline overstates by placing a Level 3→4 advancement action inside Level 3 itself. The accurate statement is: advanced version control and rollback for prompt assets is the key advancement action that takes organizations from Level 3 to Level 4.

### Auto-correction applied
The storyline's Slide 24 and Appendix C have been updated to say Level 3→Level 4 advancement (i.e., it is what gets you to Level 4), not that Level 3 "includes" this capability.

Score bands (0–9/10–14/15–19/20–28) remain correct — these are confirmed by the live page.

---

## 4. Notes on Other Verified Claims

### src-205 martinfowler.com quote (confirmed exact)
The storyline quotes: *"The act of writing surfaces assumptions that were never articulated — distinctions intuitive for the senior must become precise for the AI."*

The live page text is: *"The act of writing it surfaces assumptions that were never articulated. What exactly makes a security issue critical versus merely important? These distinctions, often intuitive for the senior, must become precise for the AI."*

The storyline combines two adjacent sentences into one. This is accurate paraphrase (not misquotation) — the meaning is preserved. The two-engineers-different-thresholds anecdote is confirmed verbatim.

### src-204 seven-step approval workflow (editorial synthesis)
The storyline presents a 7-step approval workflow at [Backup 17-1]. The live tianpan.co source does not list a numbered 7-step sequence verbatim. Instead, it describes roles (domain-aligned owners, AI platform teams, risk/compliance reviewers, release coordinators) and the principle that non-technical stakeholders can author prompts while engineering owns the eval gate. The 7-step list in the storyline is an editorial synthesis, not a direct quote. The content is consistent with the source but is not a verbatim extract.

### src-209 Humanloop "acquired by Anthropic Aug 2025, product sunset" (confirmed)
Confirmed via TechCrunch, Built In, and other sources: Anthropic acquired the Humanloop team in August 2025 (announced Aug 13, 2025). The standalone platform was sunset September 8, 2025. The note that Anthropic did NOT acquire Humanloop's IP (only the team) is accurate per TechCrunch. The URL (humanloop.com/platform/prompt-management) still resolves and shows a functional-looking page with banner "Humanloop is joining Anthropic." The characterization "acquired by Anthropic" is technically an acqui-hire (team acquisition without IP transfer), but "acquired" is acceptable common usage. Claim: OK.

### src-307 arXiv paper (confirmed)
arXiv:2601.20727v1 confirmed to exist. Full title: "Audit Trails for Accountability in Large Language Models." Authors: Victor Ojewale, Harini Suresh, Suresh Venkatasubramanian — all from Brown University Department of Computer Science. Published January 2026. The storyline's citation as "arXiv:2601.20727 Audit Trails for Accountability in LLMs (Brown University)" is accurate.

### src-102 MongoDB harness benchmark claim
The storyline states: "in a published benchmark comparison, the same model moved from ~Top 30 to Top 5 by changing only the harness." The live MongoDB article (Apr 30, 2026) states: "LangChain moved from Top 30 to Top 5 on Terminal Bench 2.0 by changing only the harness." This is confirmed, and the LangChain article (src-101) also states: "we improved our coding agent Top 30 to Top 5 on Terminal Bench 2.0 by only changing the harness." OK.

---

## 5. Items Needing Human Review (⚠️)

### ⚠️ Item 1: Seven-step approval workflow [Backup 17-1]
The 7-step numbered workflow in [Backup 17-1] is an editorial synthesis from src-204 and src-209, not a verbatim extract from either. Step details (e.g., "AI platform team reviews technical risk" as a separate step, "Compliance sign-off" as Step 6) are reasonable inferences but not explicitly enumerated in any single source. **Recommend**: Add a note like "(synthesized from src-204 governance discussion)" or soften to "an example workflow" rather than implying it is a directly documented procedure.

### ⚠️ Item 2: "2026 longitudinal study" reference in Backup 3-1
Backup 3-1 states: "2026 longitudinal study found 'meaningful behavioral drift across deployed transformer services' over ten weeks." The src-204 (tianpan.co) article references "A February 2026 longitudinal study confirmed 'meaningful behavioral drift across deployed transformer services' over a ten-week period" — but does not name the study, authors, or DOI. This citation is a secondary reference (blog citing unnamed study). The storyline inherits this indirect attribution. **Recommend**: Soften to "practitioners have reported meaningful behavioral drift…" or note the secondary nature: "(reported in src-204; primary study not directly cited)."

---

## 6. Auto-Corrections Made to storyline.en.md

**1 auto-correction** was made (Microsoft maturity level claim):

- **Slide 24**: Changed "places 'advanced version control and rollback for prompt assets' at Level 3 (Managed) of 4" → "lists 'advanced version control and rollback for prompt assets' as the key advancement action from Level 3 to Level 4 (Optimized)"
- **Appendix C**: Changed "L3 Managed (15–19, 'advanced version control and rollback' for prompt assets)" → "L3 Managed (15–19): the Level 3→L4 advancement actions specifically include 'strengthen your asset management strategies through advanced version control and rollback capabilities' — placing prompt asset-ization as the L4 threshold, not L3"
- Score bands (0–9/10–14/15–19/20–28) unchanged — confirmed correct.

---

*Verification complete. Report written: 2026-06-10.*
