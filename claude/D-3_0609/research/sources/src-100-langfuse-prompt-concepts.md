# src-100: Langfuse Prompt Management – Core Concepts (Data Model)

**URL:** https://langfuse.com/docs/prompt-management/data-model  
**Title:** Concepts – Langfuse  
**AccessDate:** 2026-06-09  
**Related section:** Prompt Asset Structure / Versioning Model

---

## Relevant Excerpt

### The Prompt Object
Langfuse considers a prompt to be a combination of both the instructions for the LLM (this can be a single string or an array of messages) and, optionally, additional configuration that influences the behavior.

**Prompt Object fields:**
- `name` – unique identifier for the prompt
- `type` – "text" (single string) or "chat" (array of role/content messages)
- `prompt` – the template body (string or array)
- `version` – sequential integer (immutable per version)
- `labels` – mutable pointers to specific versions (e.g., "production", "latest", custom labels)
- `config` – optional: model parameters, tools, response_format (versioned together with prompt)

**Text prompt example:**
```json
{
  "name": "movie-critic",
  "type": "text",
  "prompt": "As a movie critic, do you like Dune 2?",
  "version": 1
}
```

**Chat prompt example:**
```json
{
  "name": "movie-critic-chat",
  "type": "chat",
  "prompt": [
    {"role": "system", "content": "You are a movie critic."},
    {"role": "user", "content": "Do you like Dune 2?"}
  ],
  "version": 1
}
```

### Dynamic Rendering
Prompts support three ways to insert dynamic content at runtime:
| Type | Use Case |
|---|---|
| Variables | Insert dynamic text into messages |
| Prompt References | Reuse prompts across other prompts, avoid duplicating common instructions |
| Message Placeholders | Insert arrays of messages (e.g., chat history) |

### Versioning and Labels
- **Versions** provide an immutable history of every prompt change. Each update creates a new version (1, 2, 3...).
- **Labels** are pointers to specific versions. Common labels: `production`, `latest`, plus custom labels for staging, testing, A/B tests.

### Deployment Workflow
1. Create and test: Create a new prompt version (automatically gets the `latest` label)
2. Validate: Test the new version in your development environment or using the playground
3. Deploy: Update the `production` label to point to the new version
4. Monitor: Your production application automatically picks up the new version on the next fetch
5. Rollback if needed: Simply reassign the `production` label back to a previous version
