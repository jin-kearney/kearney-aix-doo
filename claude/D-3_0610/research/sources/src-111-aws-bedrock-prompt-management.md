---
URL: https://docs.aws.amazon.com/bedrock/latest/userguide/prompt-management.html
Title: Construct and store reusable prompts with Prompt management in Amazon Bedrock
AccessDate: 2026-06-10
Publisher: AWS Documentation (Amazon Web Services)
Related section: Cloud vendor prompt management, key definitions, versioning and deployment workflow
---

## Key Excerpt

### Overview
> "Amazon Bedrock provides you the ability to create and save your own prompts using Prompt management so that you can save time by applying the same prompt to different workflows."

### Key Definitions (Amazon Bedrock):
- **Prompt**: An input provided to a model to guide it to generate an appropriate response or output
- **Variable**: A placeholder that you can include in the prompt; fill in test values when testing or invoke at runtime
- **Prompt variant**: An alternative configuration of the prompt, including its message or the model or inference configurations used
- **Prompt builder**: A tool in the Amazon Bedrock console that lets you create, edit, and test prompts and their variants in a visual interface

### General Workflow:
1. Create a prompt in Prompt management that you want to reuse across different use cases; include variables to provide flexibility
2. Choose a model, inference profile, or agent; modify inference configurations
3. Fill in test values for variables and run the prompt; create variants and compare outputs to choose the best
4. Integrate the prompt into your application by:
   - Specifying when running model inference
   - Adding a prompt node to a flow

### Features:
- Version control for prompts (save versions while iterating)
- Variant comparison testing (compare different configurations side-by-side)
- Variable/placeholder support for flexible reuse
- Integration with Amazon Bedrock Flows for workflow automation
- Console-based visual prompt builder

### Deployment Pattern:
> "While iterating on your prompt, you can save versions of it. You integrate a prompt into your application with the help of Amazon Bedrock Flows."
> "To share prompts for use in downstream applications, you can create a version and make an API call to retrieve the prompt."

### Significance for D-3:
Amazon Bedrock Prompt Management is a major cloud vendor natively providing prompt asset management — this signals that prompt asset management is infrastructure, not a niche concern. The same pattern (create → test variants → version → deploy via API → reuse) appears across AWS, Azure, and Google Cloud.
