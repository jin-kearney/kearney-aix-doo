---
URL: https://www.digitalapplied.com/blog/prompt-library-team-rollout-30-60-90-day-plan-2026
Title: Prompt Library Team Rollout: 30/60/90-Day Plan 2026
AccessDate: 2026-06-10
Related section: Roadmap — phased implementation
---

## Excerpt

Published: May 15, 2026. Digital Applied Team.

### Plan overview
90-day phased roadmap from scattered prompts to versioned, eval-covered, governed library.

### Stage 1 — Days 1-30: Catalog & Versioning Foundation
**Target**: Artifacts that must exist before evals are meaningful
- Days 1-7: Catalog audit (grep + stakeholder interviews → prompt inventory CSV)
- Days 8-12: Naming + versioning convention (kebab-case slug + version field; CONVENTIONS.md)
- Days 13-20: Migrate to single source of truth (prompts/ directory, no inline strings)
- Days 21-25: Owner assignment + CODEOWNERS enforcement
- Days 26-30: README + stage-one demo + retro

**Key metric at Day 30**: catalog coverage baseline established

### Stage 2 — Days 31-60: Eval Loop & Regression Detection
**Target**: The feedback loop
- Days 31-35: Choose eval framework (Promptfoo/OpenAI Evals/DeepEval)
- Days 36-42: First eval suite (10 test cases on highest-stakes prompt)
- Days 43-48: CI integration (PR-gated eval, block merge on regression)
- Days 49-54: Regression cron + dashboard (nightly full-library run, 30-day history, owner-routed alerts)
- Days 55-60: Cross-model fit testing (top 5 prompts vs. cheaper-tier alternative)

**Key metrics at Day 60**: eval coverage %, daily regression rate, model-fit divergence

### Stage 3 — Days 61-90: Governance, Lifecycle, Training
**Target**: Social/organizational layer that makes discipline stick
- Days 61-66: Governance gates (new-prompt template: must include eval suite, owner, model-fit decision, lifecycle target)
- Days 67-72: Lifecycle policy (LIFECYCLE.md: beta → production → deprecated → archived, deletion dates)
- Days 73-78: Team training (1-hour workshop + reference deck → onboarding material)
- Days 79-84: Standing monthly governance forum (30-min meeting: review eval coverage, regression trends, deprecation queue)
- Days 85-90: Rollout handoff (team runs 1 full week independently before declaring complete)

### The 4 failure modes (most-to-least frequent)
1. Eval-before-catalog inversion (most common) — write evals before naming/versioning is done; have to rewrite all evals
2. No lifecycle policy — library accumulates zombies; nothing ever retires
3. Cron never wired — CI evals exist but nightly regression detection missing; model-vendor regressions slip through
4. Shared-channel alert routing — alerts go to team channel; nobody answers; discipline erodes

### Prompt frontmatter template (canonical fields)
```yaml
slug: customer-summary
version: 3
owner: alex.chen
model: claude-sonnet-4-6
status: production
feature: dashboard-summary
eval-suite: prompts/evals/customer-summary.yaml
last-edited: 2026-05-15
```

### Key quote
"The single biggest reason 60-day rollouts fail where 90-day rollouts succeed is that the social work has not been done — there is no written lifecycle policy, no governance forum, no training material."
