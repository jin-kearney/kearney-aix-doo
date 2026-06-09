# src-103: Humanloop – Prompts Documentation

**URL:** https://humanloop.com/docs/explanation/prompts  
**Title:** Prompts | Humanloop Docs  
**AccessDate:** 2026-06-09  
**Related section:** Prompt Asset Structure / .prompt File Format / Versioning

---

## Relevant Excerpt

### What Constitutes a Prompt Version
A Prompt on Humanloop defines the instructions and configuration for guiding a Large Language Model to perform a specific task.

Each change in any of the following properties creates a **new Version** of the Prompt:
- the **template** (e.g., `Write a song about {{topic}}`)
- the **model** (e.g., `gpt-4o`)
- the **parameters** (temperature, max_tokens, top_p)
- any **tools** available to the model

### The .prompt File Format
Humanloop defined a `.prompt` file format — a serialized, human-readable representation of a Prompt Version, suitable for integration into version control systems alongside code. Format uses YAML frontmatter + JSX-style template body:

```
---
model: gpt-4o
temperature: 1.0
max_tokens: -1
provider: openai
endpoint: chat
---
<system>
Write a song about {{topic}}
</system>
```

### Input Variables
Inputs are defined in the template through the double-curly bracket syntax (e.g., `{{topic}}`). The value of the variable must be supplied when the Prompt is called to create a generation.

"This separation of concerns, keeping configuration separate from the query time data, is crucial for enabling you to experiment with different configurations and evaluate any changes."

### Versioning Principle
A Prompt File will have multiple Versions as you iterate on different models, templates, or parameters, but each version should perform the same task and generally be interchangeable with one another.

One Prompt File = one task (e.g., "Writing Copilot", "Personal Assistant", "Summarizer"). Create a new Prompt File for each different 'task to be done'.

### Note on Humanloop Status
HumanLoop shut down on September 8, 2025. However, their `.prompt` file format remains an important industry reference for structured prompt serialization.
