# src-201 — Langfuse: Prompt Version Control

**URL:** https://langfuse.com/docs/prompt-management/features/prompt-version-control  
**Title:** Version Control — Langfuse Docs  
**AccessDate:** 2026-06-10  
**Related section:** How-3 (Version Management)

---

## Relevant Excerpt

In Langfuse, version control & deployment of prompts is managed via `versions` and `labels`.

**Versions & Labels**  
Each prompt version is automatically assigned a `version ID`. Additionally, you can assign `labels` to follow your own versioning scheme.

Labels can be used to assign prompts to environments (staging, production), tenants (tenant-1, tenant-2), or experiments (prod-a, prod-b).

**Fetching by Label or Version**  
To "deploy" a prompt version, you have to assign the label `production` or any environment label you created to that prompt version.

- The `latest` label points to the most recently created version.
- When using a prompt without specifying a label, Langfuse will serve the version with the `production` label.

**Rollbacks**  
When a prompt has a `production` label, then that version will be served by default in the SDKs. You can quickly rollback to a previous version by setting the `production` label to that previous version in the Langfuse UI.

**Prompt Diffs**  
The prompt version diff view shows you the changes you made to the prompt over time. This helps you understand how the prompt has evolved and what changes have been made to debug issues or understand the impact of changes.

**Protected Prompt Labels**  
Protected prompt labels give project admins and owners the ability to prevent labels from being modified or deleted, ensuring better control over prompt deployment. Once a label such as `production` is marked as protected, `viewer` and `member` roles cannot modify or delete the label from prompts.

**Prompt Composability**  
Langfuse supports referencing other prompts inside a prompt using a special syntax: `(@@@langfusePrompt:name=PromptName|label=production@@@)`.

**Features Overview (from Nearform review):**
- HTTP API with OpenAPI spec + Python/JS SDKs
- RBAC: 5 predefined roles (Owner, Admin, Member, Viewer, None)
- Automatic versioning on every update
- Labels for environments (staging, production)
- Diff view for comparing versions
- Caching support for SDK layer
- 16.9k GitHub stars; MIT Expat license; Y Combinator backed
