# src-205 — Encoding Team Standards (MartinFowler.com)

**URL:** https://martinfowler.com/articles/reduce-friction-ai/encoding-team-standards.html  
**Title:** Encoding Team Standards  
**Author:** Rahul Garg (Principal Engineer, Thoughtworks)  
**Published:** 2026-03-31  
**AccessDate:** 2026-06-10  
**Related section:** How-1 (Capture & Convert), How-2 (Structuring), How-7 (Governance)

---

## Relevant Excerpt

### The Core Problem
"When a team has worked together long enough, certain practices become invisible. The senior engineer who rejects a pull request does not consult a checklist; she recognizes, almost instantly, that the error handling is incomplete... Ask her to explain and she can, but in the moment it is pattern recognition built from years of reviews, production incidents, and architectural discussions. This tacit knowledge... is the team's most valuable and most fragile asset. It lives in people's heads, transfers slowly through pairing and code review, and walks out the door when someone leaves."

"Two developers on the same team, using the same tool, same codebase, same project context, producing materially different results. The gap is not in what the AI knows about the project. It is in what the AI is told to *do* with that knowledge."

### Executable Governance — Two Moves
1. **From tacit to explicit**: The target format is not a wiki page or a checklist, but a structured instruction set that an AI can execute. "The act of writing it surfaces assumptions that were never articulated. What *exactly* makes a security issue critical versus merely important?"
2. **From documentation to execution**: "Linting rules are versioned config files, not personal preferences. CI/CD pipelines are executable definitions, not wiki pages describing deployment steps. AI instructions belong in the same category: configuration that executes, not documentation that informs."

### Anatomy of a Well-Structured Executable Instruction (4 elements)
1. **Role definition**: Not for persona, but sets expertise level and perspective
2. **Context requirements**: What the instruction needs before it can operate — relevant code, architectural context, constraints. Makes dependencies explicit
3. **Categorized standards**: The categories matter more than individual items:
   - For generation: architectural compliance (must follow) / convention adherence (should follow) / style preferences (nice to have)
   - For security: critical vulnerabilities (blockers) / important concerns (must address before merge) / advisories (track and evaluate)
   - "This priority structure encodes the team's *judgment*, not just their knowledge."
4. **Output format**: Structured response with summary, categorized findings, clear next steps. "Ensures output from the instruction is comparable across runs and across developers."

### Surfacing Tacit Knowledge — The Extraction Interview
"Structured around pointed questions that span the full development workflow: What architectural decisions should never be left to individual judgment? Which conventions are corrected most often in generated code? Which security checks are applied instinctively? What triggers an immediate rejection in review? What separates a clean refactoring from an over-engineered one?"

"The answers map directly to instruction structures. Non-negotiable architectural patterns become generation constraints. Frequent corrections become convention checks. Security instincts become threat-model items. Review rejections become critical checks."

Real case: "The extraction conversation revealed that two senior engineers had quietly different thresholds for what counted as a 'critical' security concern versus an 'important' one, a disagreement that had never surfaced because each reviewed different pull requests. The act of writing a shared instruction forced the distinction to be made explicit."

### Standards as Shared Infrastructure
"A prompt on an individual machine is a personal productivity hack. The same prompt in the team's repository is infrastructure. The distinction is the difference between a personal preference and a team practice."

When standards live in the repository:
- Changes tracked, standards owned collectively
- Every developer working from the same version
- Updated through pull requests like any other infrastructure change
- "The standards are not static rules handed down by a senior and frozen in place. They are living artifacts that the whole team maintains."

### Calibration / When to Use
"If AI-assisted output visibly varies in quality depending on who is prompting, or if generation and review work is routing through a handful of people because they are the only ones who know how to prompt effectively, the inconsistency is the signal. Teams of five may not need this. Teams of fifteen almost certainly do."

### Implementation Example: Lattice
[Lattice](https://github.com/techygarg/lattice) encodes this practice as composable atoms — `clean-code`, `architecture`, `domain-driven-design`, `secure-coding`, `test-quality` — each with self-validation checklists. Refiners let teams customize defaults through a guided interview, producing a versioned standards document.
