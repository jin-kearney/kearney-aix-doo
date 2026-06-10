# src-208 — From Prompts to Harnesses — Four Years of AI Agentic Patterns

**URL:** https://bits-bytes-nn.github.io/insights/agentic-ai/2026/04/05/evolution-of-ai-agentic-patterns-en.html  
**Title:** From Prompts to Harnesses — Four Years of AI Agentic Patterns  
**Author:** Jonas Kim  
**Published:** 2026-04-05  
**AccessDate:** 2026-06-10  
**Related section:** How-6 (Harness Packaging)

---

## Relevant Excerpt

### The Three-Era Arc
"Between 2022 and 2026, the AI development paradigm shifted three times: Prompt Engineering → Context Engineering → Harness Engineering."

- **Prompt Engineering (2022-2024)**: "What should I say?" — We believed the quality of instructions sent to the model determined success or failure.
- **Context Engineering (2025)**: "What information should I provide?" — We realized what fills the context window matters more than the prompt itself.
- **Harness Engineering (2026)**: "What system should I build?" — We accepted that the design of the entire system consuming context is the real problem.

Epsilla's metaphor: "In 2022, we studied how to write the perfect email. In 2025, we learned to manage our inbox. In 2026, we're designing the email system itself."

### Key Thesis: Rigor Relocates, Not Disappears
"Engineering rigor never disappeared. It relocated." — From writing code to designing context, from designing context to system architecture. Same principle as when XP moved rigor from design documents to automated tests.

### Prompt Engineering Era Limits ("Blind Prompting")
Mitchell Hashimoto called this "Blind Prompting" — writing prompts through trial and error, without rigorous measurement or testing. "Tweak the prompt, eyeball the output, conclude 'looks okay this time.' Closer to alchemy than software engineering."

The problem was structural: models are non-deterministic. One report says adding "Please" improved performance; elsewhere, a single newline completely changes output.

### Harness Definition
A harness is a structured workflow engine that wraps AI models in enough scaffolding to make them reliable, auditable, and repeatable in production environments.

**Harness Components:**
- Prompt (instructions for the model)
- Model connector
- Input data contract
- Tool-call ordering and configuration (MCP servers)
- Output format specification
- Evaluation criteria linkage
- Phase gates and handoff artifacts

### Worker Agent Pattern (from Harness.io)
"Worker Agents are AI-powered automation units that execute tasks inside Harness pipelines using a language model, MCP-connected data sources, and configurable inputs. Each Worker Agent pairs a prompt (Instructions), a Model Connector, and optional MCP Servers into a single, reusable, governed step."

### Why Prompts Alone Failed
"A team spends three weeks polishing their coding agent's prompt... Then the project scales up, and things break. The agent starts ignoring existing utility functions... No matter how many times you write 'reuse existing code' in the prompt, if the utility file isn't in the context window, the agent doesn't know it exists. The prompt was perfect, but the information the agent could see was incomplete. This isn't a prompt problem. It's a context problem."

### Implication for Prompt Asset-ization
Prompts must be packaged as part of harnesses — bundled with their context requirements, tool dependencies, input data contracts, and expected output format. A prompt divorced from its execution context is incomplete as a managed asset.
