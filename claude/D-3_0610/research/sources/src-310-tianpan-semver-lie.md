# src-310 — The Semver Lie: Why a Minor LLM Update Breaks Production

- **URL:** https://tianpan.co/blog/2026-04-29-semver-lie-llm-minor-update-breaks-production
- **Title:** The Semver Lie: Why a Minor LLM Update Breaks Production More Reliably Than a Major Refactor
- **AccessDate:** 2026-06-10
- **Related section:** Why (dependency/fragility argument — model version upgrades as breaking changes)

---

## Key Excerpts (from search result summary)

> "Model updates can result in breaking changes such as changing format, reasoning style, or tool-call ordering. More provocatively, **every model bump is a breaking change in disguise**, with the version number and patch level not mattering."

> "When you deploy traditional software, you control every dependency, but with LLM APIs, the model is a black box maintained by someone else. **Prompts are hypotheses about how a specific model will interpret instructions**, and when the model changes, the hypothesis must be revalidated."

> "Prompts regress more on unspecified requirements across model updates, with an almost **2x increase** compared to specified requirements." (Citing: "When 'Better' Prompts Hurt" arXiv:2601.22025)

### Core Structural Argument

The semantic versioning (semver) contract that software engineers rely on — where patch updates are safe, minor updates add features, major updates break APIs — **does not hold for LLM models**. A "minor" model update (e.g., GPT-4o patch) can change output formats, reasoning verbosity, or tool-call ordering in ways that silently break downstream business logic encoded in prompts.

---

*Source: TianPan.co engineering blog. Published Apr 29, 2026. Accessed 2026-06-10.*
