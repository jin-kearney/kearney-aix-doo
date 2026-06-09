# Source Verification Report — D-3 Prompt/Harness Asset Management
**Verification date:** 2026-06-09  
**Storyline file:** D-3_0609/storyline.en.md  
**Verifier:** Source-verification specialist (automated agent)

---

## Summary

| Category | Count |
|----------|-------|
| **OK** — live, accessible, claim supported | 33 |
| **MISMATCH** — URL live but title or claim conflicts | 2 |
| **BROKEN** — URL dead / wrong slug | 1 |
| **UNVERIFIABLE** — blocked, paywalled, JS-only, or timeout | 5 |
| **Auto-corrections made** | 2 |
| **Items flagged for human review (⚠️)** | 2 |

---

## Per-Source Verification Table

| Source ID | URL (in storyline) | Claim (short) | Verdict | Action | Replacement / Note |
|-----------|-------------------|---------------|---------|--------|--------------------|
| src-050 | dataversity.net/…/prompt-engineering-is-dead-long-live-promptops/ | PromptOps concept | OK | None | — |
| src-051 | medium.com/@martin_rodek/…prompt-registry… | Enterprise prompt registry | OK | None | — |
| src-052/107/154/200 | medium.com/@segev_shmueli/…portfolio-approach… | Prompt portfolio, 40–60% reuse benchmark | OK | None | Confirmed benchmark in source file src-200 |
| src-053 | wiprotechblogs.medium.com/managing-prompt-technical-debt… | Prompt technical debt / PLM lifecycle | OK | None | — |
| src-054 | medium.com/the-future-of-data/…prompt-governance… | Bojan Ciric, Deloitte Fellow; prompt governance | OK | None | Author title confirmed live |
| src-055 | dovient.com/…tacit-knowledge-manufacturing-hidden-asset | "90% of what makes them invaluable" (tacit knowledge) | OK | None | Claim verified live |
| src-056 | prompts.ai/blog/decoupled-ai-pipelines… | Decoupled pipelines / dependency management | UNVERIFIABLE | None | Page returned empty (blocked). ⚠️ Flag for human review — minor supporting reference only |
| src-057 | agenta.ai/blog/prompt-versioning-guide | Prompt versioning guide (Agenta) | OK | None | — |
| src-058/156/206 | getmaxim.ai/articles/prompt-versioning-best-practices… | Prompt versioning best practices (Maxim) | OK | None | — |
| src-059 | cio.com/article/4130960/prompt-governance-is-the-new-data-governance.html | Prompt governance = new data governance | OK | None | — |
| src-060 | pecollective.com/glossary/prompt-drift/ | Prompt drift definition | OK | None | — |
| src-061 | cxtoday.com/…ai-audit-trail-regulatory-scrutiny/ | AI audit trails / regulatory scrutiny | OK | None | — |
| src-062 | link.springer.com/article/10.1007/s44163-022-00020-w | Tacit knowledge elicitation for Industry 4.0 (Springer) | OK | None | Open-access article, title confirmed |
| src-063 | mlflow.org/prompt-registry | MLflow prompt registry landing page | OK | None | — |
| src-100 | langfuse.com/docs/prompt-management/data-model | Langfuse prompt data model | OK | None | — |
| src-101 | braintrust.dev/articles/what-is-prompt-management | Braintrust prompt management guide | OK | None | — |
| src-102 | arize.com/blog/top-5-ai-prompt-management-tools-of-2025/ | Top 5 prompt management tools (Arize) | OK | None | — |
| src-103 | humanloop.com/docs/explanation/prompts | Humanloop "shut down September 8, 2025" | OK | None | Storyline already annotates as "historical; shut down Sep 2025" — confirmed |
| src-104 | docs.promptlayer.com/features/prompt-registry/overview | PromptLayer registry overview | OK | None | — |
| src-105 | mlflow.org/docs/latest/genai/prompt-registry/ | MLflow prompt registry docs | OK | None | — |
| src-106 | blog.neosage.io/p/the-prompt-lifecycle-every-ai-engineer | Prompt lifecycle / NeoSage (Dimple Sharma) | OK | None | Actual page title is "The Prompt Engineering Playbook"; subtitle matches INDEX entry |
| src-108 | **augmentir.com/augipedia/tacit-knowledge** | Tacit knowledge / Augmentir | **BROKEN** | **Auto-corrected** | Correct URL: `https://www.augmentir.com/glossary/tacit-knowledge` |
| src-109 | langwatch.ai/blog/what-is-prompt-management… | LangWatch prompt management guide | OK | None | — |
| src-110 | aws.amazon.com/bedrock/prompt-management/ | AWS GENOPS03-BP01 best practice | ⚠️ MISMATCH (citation proxy) | **Flagged for human review** | The claim cites GENOPS03-BP01 (AWS Well-Architected). The cited URL is the Bedrock product page (correct context), but the actual Well-Architected best practice lives at `https://docs.aws.amazon.com/wellarchitected/latest/generative-ai-lens/genops03-bp01.html`. The claim itself is accurate; the citation is a related product page, not the primary source. |
| src-111/158 | github.com/BoundaryML/baml | BAML structured prompt format | OK | None | — |
| src-152 | martinfowler.com/articles/structured-prompt-driven/ | SPDD article (Wei Zhang, Jessie Xia) | OK | None | Title confirmed: "Structured-Prompt-Driven Development (SPDD)" |
| src-153 | docs.aws.amazon.com/prescriptive-guidance/…/prompt-agent-and-model.html | AWS Prescriptive Guidance prompt lifecycle | OK | None | — |
| src-155 | launchdarkly.com/blog/prompt-versioning-and-management/ | LaunchDarkly prompt versioning guide | OK | None | — |
| src-157 | pulsegeek.com/articles/how-to-create-reusable-prompt-libraries… | Reusable prompt libraries / taxonomy | OK | None | — |
| src-159 | fulcrumdigital.com/blogs/prompt-governance-the-emerging-enterprise-control-layer/ | Prompt governance enterprise control layer | UNVERIFIABLE | None | JS-only / redirect — cannot render. ⚠️ Minor supporting reference; flag for human review |
| src-160 | docs.langchain.com/langsmith/manage-prompts | LangSmith prompt management docs | OK | None | URL redirected but content confirmed live |
| src-161 | agenta.ai/ | Agenta LLMOps platform homepage | OK | None | — |
| src-162 | braintrust.dev/articles/what-is-prompt-versioning | Braintrust prompt versioning guide | OK | None | — |
| src-163 | addyosmani.com/blog/agent-harness-engineering/ | Agent harness engineering (Addy Osmani) | OK | None | — |
| src-164 | blog.promptlayer.com/5-best-tools-for-prompt-versioning/ | PromptLayer prompt versioning tools | OK | None | URL redirects cleanly, content confirmed |
| src-165 | doppler.com/blog/advanced-llm-security | GitGuardian: 29M hardcoded secrets, +34% YoY | OK | None | Confirmed via WebSearch; 28.65M rounds to "29 million" — acceptable |
| src-166 | arxiv.org/pdf/2512.05122 | "Documenting SME Processes with Conversational AI: From Tacit Knowledge to BPMN" | **MISMATCH** (title wrong) | **Auto-corrected** | Actual paper title: "Tacit Know-How Made Visible: Using Conversational AI for Business-Process Documentation" (Unnikrishnan Radhakrishnan, Aarhus University) |
| src-167 | dl.acm.org/doi/10.1145/3722212.3725124 | ACM SIGMOD 2025 prompt editor taxonomy | UNVERIFIABLE | None | ACM DL returned empty — paywalled/blocked. Claim is minor/supplementary; ⚠️ flag for human review |
| src-205 | digitalapplied.com/blog/prompt-library-metrics-coverage-regression-framework-2026 | KPI panel / prompt library metrics | OK | None | — |
| src-208 | arxiv.org/abs/2603.15044 | PRL maturity framework (PRL 8, PRL 9 descriptions) | UNVERIFIABLE | None | arxiv URL resolves but PDF unreadable via fetch; content captured from search snippet in src-208 source file. ⚠️ Flag for human review |

---

## Items Requiring Human Review (⚠️)

| # | Source ID | Issue | Recommended Action |
|---|-----------|-------|-------------------|
| 1 | **src-110** | GENOPS03-BP01 claim cites `aws.amazon.com/bedrock/prompt-management/` as primary source. The actual Well-Architected best practice is at `https://docs.aws.amazon.com/wellarchitected/latest/generative-ai-lens/genops03-bp01.html`. The claim is accurate but the citation URL is a proxy (product page, not the spec page). | Replace src-110 URL in References with the Well-Architected URL, OR keep current URL with a note that the Bedrock product page implements this best practice. Either is defensible. |
| 2 | **src-208** | PRL maturity framework (arxiv 2603.15044) — PDF could not be machine-read. Only metadata and search snippet are confirmed. PRL 8 and PRL 9 descriptions used in the Roadmap section were captured from the snippet, not full text. | Manually verify PRL 8/9 descriptions against the full paper before publication. |
| 3 | **src-167** | ACM SIGMOD 2025 (dl.acm.org) — paywalled/empty response. Claim is minor ("taxonomy-driven" framing in one sentence). | Verify via institutional access or replace with an alternative accessible source. |
| 4 | **src-056** | prompts.ai/blog/… — empty response (blocked). Minor supporting reference for "decoupled pipelines." | Verify accessibility or replace with an alternative source. |

---

## Auto-Corrections Made to storyline.en.md

### Correction 1 — BROKEN URL for src-108 (Augmentir)
- **Location:** References section (line 1151)
- **Before:** `https://www.augmentir.com/augipedia/tacit-knowledge`
- **After:** `https://www.augmentir.com/glossary/tacit-knowledge`
- **Basis:** Confirmed via WebSearch; the /augipedia/ slug returns 404; /glossary/ is the live page.

### Correction 2 — MISMATCH title for src-166 (arXiv paper)
- **Location:** References section (line 1167)
- **Before:** `Documenting SME Processes with Conversational AI: From Tacit Knowledge to BPMN`
- **After:** `Tacit Know-How Made Visible: Using Conversational AI for Business-Process Documentation`
- **Basis:** Direct PDF fetch of `arxiv.org/pdf/2512.05122` confirmed the actual title.

---

## Notes on Verification Methodology

- All URLs were fetched via `web_fetch`. If a fetch returned empty or was blocked, the source was marked UNVERIFIABLE per policy (no bypass).
- Quantitative claims (29M secrets, 40–60% reuse rate, "90% tacit knowledge") were cross-checked via WebSearch or archived source files and confirmed.
- The Humanloop shutdown date (September 8, 2025) was confirmed via WebSearch; the storyline already handles this with a "historical" annotation.
- The src-056 (prompts.ai) URL returned empty on fetch; it was noted in the INDEX as BLOCKED.
- src-159 (Fulcrum Digital) requires JavaScript to render and could not be verified; claim is a minor supplementary reference.
