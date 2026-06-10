# src-210 — SOP-to-Prompt Conversion and Knowledge Elicitation

**URL (primary):** https://klariti.com/2025/04/14/getting-started-guide-to-prompt-engineering-for-sops/  
**Supporting URL:** https://arxiv.org/html/2604.28043v1 (CARE methodology — Collaborative Agent Reasoning Engineering)  
**Supporting URL 2:** https://klariti.com/2025/04/15/10-ptcf-prompt-examples-for-sops/  
**Title:** Getting Started Guide to Prompt Engineering for SOPs  
**AccessDate:** 2026-06-10  
**Related section:** How-1 (Capture & Convert)

---

## Relevant Excerpt

### PTCF Structure for SOPs
To help SOP writers maximize the potential of prompt engineering, the core structure of an effective prompt is broken down into four essential components:

1. **Persona**: Defines the voice or expertise the AI should adopt (e.g., "You are a quality assurance engineer with 10 years of experience in automotive manufacturing")
2. **Task**: Describes the exact action to perform
3. **Context**: Grounds the prompt in a real-world scenario or operational setting
4. **Format**: Ensures the output aligns with professional SOP standards

### Decomposed Prompting
"A modular approach designed to tackle complex tasks by breaking them down into simpler, manageable sub-tasks... Decomposition tasks the LLM with breaking the larger problem into sub-problems, solving the first sub-problem, and appending its solution back to the main prompt with all sub-problems."

### CARE Methodology for SME Knowledge Elicitation
From arxiv.org/html/2604.28043v1 — Collaborative Agent Reasoning Engineering (CARE):
- **Phase-aligned elicitation**: Helper agents run structured interviews generating targeted, phase-aligned questions
- **Artifact drafting**: Draft initial scoped artifact sets (constraints, decomposition targets, intended users) to seed SME and developer review
- **Three-party design**: Subject Matter Experts, Developers, and Helper Agents collaborate on reasoning engineering
- **Iterative validation**: Generated questions and draft artifacts are validated through human review loops

### Converting Tacit Knowledge
Key insight from tacit knowledge literature (phpkb.com/kb/article/capturing-and-converting-tacit-knowledge-343.html):
- "Codification involves transforming tacit knowledge into explicit knowledge that can be documented and shared"
- "Revising existing SOPs by incorporating annotations based on captured tacit knowledge could include adding explanations for specific steps, decision trees for complex situations, or a dedicated 'Best Practices' section based on employee insights"
- Interactive iterative knowledge acquisition uses a combination of procedural, storytelling, and salesman techniques

### Manufacturing Quality Inspection Example (SOP → Prompt Decomposition)
From quality inspection SOP research:

**Original SOP instruction (tacit, vague):**
"Inspect for defects"

**Structured explicit version (suitable for prompt conversion):**
"Check for scratches longer than 5mm, cracks of any size, discoloration covering more than 10% of surface area. Reference defect classification guide VS-002 for borderline cases. Record findings on Quality Log QF-004."

**Prompt decomposition approach:**
1. Role: "You are a quality control inspector with expertise in [product type]"
2. Input contract: Product ID, inspection batch number, reference standard version
3. Judgment steps: Enumerate each defect category with specific measurable thresholds
4. Decision rules: If-then structure for severity classification (Critical/Major/Minor)
5. Reference criteria: Link to defect classification guide as few-shot examples
6. Prohibited actions: Do not pass ambiguous cases without escalation
7. Output format: JSON with fields {defect_type, severity, location, measurement, action_required}

### From MartinFowler.com Article (cross-reference src-205)
The extraction interview questions for surfacing tacit judgment:
- What architectural/procedural decisions should never be left to individual judgment?
- Which conventions are corrected most often in AI-generated output?
- Which checks are applied instinctively by experienced staff?
- What triggers an immediate rejection/escalation?
- What separates a good output from an over-engineered one?
