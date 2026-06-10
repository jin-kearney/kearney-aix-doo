---
URL: https://www.langchain.com/blog/the-anatomy-of-an-agent-harness
Title: The Anatomy of an Agent Harness
AccessDate: 2026-06-10
Author: Vivek Trivedy (LangChain)
Published: March 10, 2026
Related section: Harness definition, harness components, MECE boundary
---

## Key Excerpt

**Definition:**
> "A harness is every piece of code, configuration, and execution logic that isn't the model itself."
>
> "Agent = Model + Harness. If you're not the model, you're the harness."

A raw model is not an agent. It becomes one when a harness gives it: state, tool execution, feedback loops, and enforceable constraints.

**Concrete harness includes:**
- System Prompts
- Tools, Skills, MCPs + and their descriptions
- Bundled Infrastructure (filesystem, sandbox, browser)
- Orchestration Logic (subagent spawning, handoffs, model routing)
- Hooks/Middleware for deterministic execution (compaction, continuation, lint checks)

**Core harness components (derived from desired agent behaviors):**
1. Filesystems for durable storage and context management
2. Bash + Code as a general purpose tool
3. Sandboxes and Tools to Execute & Verify Work
4. Memory & Search for continual learning
5. Battling Context Rot (compaction, tool call offloading, skills)
6. Long Horizon Autonomous Execution (planning, self-verification)

**On system prompts as harness:**
> "The harness constructs the full input: system prompt + tool schemas + memory files + conversation history + current user message."

**Key insight:**
> "Harness Engineering is how we build systems around models to turn them into work engines. The model contains the intelligence and the harness makes that intelligence useful."

**Future directions:**
- Harnesses that dynamically assemble the right tools and context just-in-time for a given task instead of being pre-configured
- Agents that analyze their own traces to identify and fix harness-level failure modes

**Note on harness vs agent framework:**
Skills are a harness level primitive for progressive disclosure of tools to avoid context rot. The harness engineering concept is broader than any specific agent framework — it refers to the surrounding system design regardless of which framework implements it.
