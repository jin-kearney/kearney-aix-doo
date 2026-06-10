---
URL: https://docs.langchain.com/langsmith/manage-prompts
Title: Manage prompts — Docs by LangChain (LangSmith Official Documentation)
AccessDate: 2026-06-10
Publisher: LangChain (LangSmith)
Related section: Prompt registry features, versioning, environment management, RBAC
---

## Key Excerpt

### Overview
LangSmith provides several tools to manage prompts effectively:
- **Environments** for promoting commits through Staging and Production
- **Commit tags** for version control and environment management
- **Prompt owners** for controlling who can promote commits and delete a prompt
- **Webhook triggers** for automating workflows when prompts are updated
- **Public prompt hub** for discovering and using community-created prompts

### Environments (dev/staging/production)
Environments represent named deployment targets (Staging and Production) assigned to specific commits. They let you track which version of a prompt is active in each environment and promote commits between them.

Environments are defined by **reserved commit tags** (`staging` and `production`) managed through the promotion UI rather than the freeform tag picker.

**To promote a commit:** Hover over a commit → Promote → Select Staging or Production.

**Rollback:** Each environment maintains an ordered history of which commits were assigned to it and when.

### Commit Tags
Commit tags are labels that reference a specific commit in prompt version history. By referencing tags rather than commit IDs in code, you can update which version is being used without modifying the code itself.

Each tag references exactly one commit.

```python
# Pull a prompt by tag
prompt = client.pull_prompt("joke-generator:production")
# Equivalent to:
prompt = client.pull_prompt("joke-generator:a1b2c3d4")
```

### Prompt Owners (RBAC)
Fine-grained control over who can tag commits and delete prompts.

**Two access modes:**
- **Workspace authorized users** (default): any user with `prompts:tag` permission can create/update/delete tags
- **Owners only**: only users added as prompt owners can create/update commit tags, promote commits, and delete the prompt

LangSmith automatically adds the prompt creator as an owner.

### Webhook Triggers
A webhook can be triggered whenever a commit is made to a prompt. Use cases:
- Triggering a CI/CD pipeline when prompts are updated
- Synchronizing prompts with a GitHub repository
- Notifying team members about prompt modifications

**Webhook payload fields:**
- `prompt_id` — ID of the prompt
- `prompt_name` — name of the prompt
- `commit_hash` — commit hash
- `created_at` — date of commit
- `created_by` — author
- `manifest` — the manifest of the prompt

### Public Prompt Hub
Community-created prompts. Search by name, handle, use cases, descriptions, or models. Fork prompts to personal organization.

**Note:** Prompts are user-generated and unverified. LangChain does not review or endorse public prompts.
