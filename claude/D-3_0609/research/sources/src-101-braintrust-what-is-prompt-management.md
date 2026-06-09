# src-101: Braintrust – What is Prompt Management?

**URL:** https://www.braintrust.dev/articles/what-is-prompt-management  
**Title:** What is prompt management? Versioning, collaboration, and deployment for prompts  
**AccessDate:** 2026-06-09  
**Related section:** Prompt Asset Definition / Lifecycle / Registry Architecture

---

## Relevant Excerpt

### What Prompt Management Is
Prompt management is the practice of treating prompts as production assets instead of static text. It includes versioning, testing, deployment, and monitoring so prompt changes can be made safely, tracked clearly, and rolled back when needed.

**Distinction from prompt engineering:**
- Prompt engineering focuses on writing prompts that produce better outputs (instructional design, examples, formatting).
- Prompt management focuses on how prompts are handled in production: version control, collaboration, staged deployment, evaluation, and monitoring.

### Components of a Production Prompt
A production prompt consists of several components that must be managed together:
- Instructions defining the task
- Context providing supporting information
- Variables enabling dynamic inputs
- Model settings (temperature, max tokens)

### How Prompt Versioning Works
- **Unique version identifiers:** Each prompt version receives a unique identifier when saved.
- **Version immutability:** Saved prompt versions never change. Any modification creates a new version instead of overwriting an existing one.
- **Diff visibility:** Teams can compare prompt versions side by side.
- **Environment separation:** Prompt versions progress through development, staging, and production as distinct stages.

### Cross-functional Collaboration Capabilities Table
| Capability | Purpose |
|---|---|
| Review workflows | Ensure prompt changes are reviewed and approved before deployment |
| Role-based access control | Separate view, edit, and deploy permissions |
| Audit trails | Record every prompt change, author, timestamp, content history |
| Shared libraries | Provide reusable prompt templates for common instructions |
| Unified workspaces | Enable engineers, product teams, domain experts to collaborate |
| Bidirectional synchronization | Keep prompt changes in code and UI in sync |

### Reference Architecture for Prompt Management Systems
A complete prompt management system connects six components:
1. Registry (centralized versioned store)
2. Evaluation engine
3. Deployment controller
4. Observability and tracing layer
5. Rollback controls
6. Feedback loops (production traces → new test cases)

### Key Problem Solved: Version Chaos
Without a tracking system, multiple versions of the same prompt spread across repositories, documents, and chat threads. When an AI feature breaks in production, engineers often spend hours trying to identify which version is actually running.

### Decoupling from Application Code
Instead of embedding prompts directly in application code, production systems fetch active prompts from a centralized registry at runtime. Prompt updates become operational changes that skip full code deployments.
