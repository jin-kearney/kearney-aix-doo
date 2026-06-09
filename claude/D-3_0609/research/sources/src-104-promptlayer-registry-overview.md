# src-104: PromptLayer – Prompt Registry Overview

**URL:** https://docs.promptlayer.com/features/prompt-registry/overview  
**Title:** Prompt Registry Overview – PromptLayer  
**AccessDate:** 2026-06-09  
**Related section:** Registry Features / Metadata / Labels / A/B Testing

---

## Relevant Excerpt

### Registry Definition
The Prompt Registry allows you to easily manage your prompt templates, which are customizable prompt strings with placeholders for variables. Viewed as a "Prompt Management System (CMS)", this registry allows your org to pull out and organize the prompts that are currently dispersed throughout your codebase.

### Template Structure
A prompt template is your prompt string with variables indicated in curly brackets (e.g., `This is a prompt by {author_name}`). Prompt templates:
- Can have tags
- Are uniquely named
- Support two variable syntaxes: f-string `{variable}` or Jinja2 `{{variable}}`

### Core Registry Features
1. **Release Labels** – mutable labels (e.g., `prod`, `staging`) attached to template versions; used to retrieve specific versions at runtime. Labels must be unique across all versions.
2. **Commit Messages** – 72-char descriptions on each template version (like Git commit messages).
3. **Custom Metadata** – key-value pairs associated with individual prompt template versions (e.g., `provider`, `model`, `temperature`, `category`).
4. **Tags** – for categorization and search.
5. **Threaded Comments** – collaborative discussions on prompt versions.
6. **Version History** – track average score, latency, and cost per template version.

### Fetching Prompts at Runtime
Applications pull prompts from registry at runtime:
- By release label: e.g., fetch `prod` version
- By version number: fetch specific version
- Default: newest version returned

### A/B Testing via Registry
With A/B Releases:
1. Create multiple versions of a prompt template
2. Use Dynamic Release Labels to split traffic between versions (by percentage or user segment)
3. Compare quality metrics across variants before committing to a change

### Collaboration Value
"Engineering teams waste cycles deploying prompts and content teams are often blocked waiting on these deploys. By programmatically pulling down prompt templates at runtime, product and content teams can visually update & deploy prompt templates without waiting on eng deploys."

### Tracking and Analytics
Associate requests with the template they used → track average score, latency, and cost per prompt template version.
