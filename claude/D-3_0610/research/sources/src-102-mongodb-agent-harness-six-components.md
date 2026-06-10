---
URL: https://www.mongodb.com/company/blog/technical/agent-harness-why-llm-is-smallest-part-of-your-agent-system
Title: The Agent Harness: Why the LLM Is the Smallest Part of Your Agent System
AccessDate: 2026-06-10
Authors: Mikiko Bazeley, Ashish Kumar, Charlie Xu (MongoDB)
Published: April 30, 2026
Related section: Harness definition, six components, MECE with MLOps analogy
---

## Key Excerpt

**The Sculley diagram analogy:**
> The LLM sits at the center of a production agent system, the same way ML code sat at the center of a production ML system — important, but tiny relative to the infrastructure that makes it work.

The famous 2015 Google NeurIPS paper "Hidden Technical Debt in Machine Learning Systems" (Sculley et al.) showed ML code as a tiny black box surrounded by vast infrastructure: data collection, feature extraction, serving, monitoring, configuration.

**Six harness components:**
1. **State and persistence** — stores and retrieves execution context, checkpoints, intermediate outputs, agent position within multi-step task
2. **Security and governance** — controls what agent is permitted to do, identity propagation, permission scoping, auditability
3. **Orchestration and tool use** — drives the agent loop (ReAct: reason → action → observe → repeat), tool calls to external systems
4. **Memory** — two levels: agent-side working memory (context window) and harness-side persistent store (episodic, conversation, entity stores via vector index)
5. **Observability** — records what agent did, decided, used, and what happened; fundamentally different from application monitoring
6. **Evals** — run out-of-band, determine whether agent is ready to deploy and whether it should remain deployed

**Harness vs. platform distinction:**
> "Where a harness manages one agent system as one coordinated runtime, the platform provides the infrastructure that makes many harnesses operable across many teams over time."

**Platform layer four properties:**
1. Durable execution — keeps work intact through process crashes/pauses
2. Governance — propagates identity and produces audit evidence automatically
3. Cost visibility — surfaces spend at agent and action level
4. Data integration — reaches enterprise systems without requiring migration

**Three maturity stages:**
| Stage | What it looks like | What breaks first |
|-------|-------------------|-------------------|
| 1. Prompt wrapper | LLM API + thin app layer | Anything needing context across sessions |
| 2. Stateful orchestration | Context delivery, memory, multi-step coordination | Cost attribution, audit evidence, multi-team |
| 3. Governed fleet | Platform layer + durable execution + identity | Almost no team is here yet |

**Key ratio finding:**
> "A governed agent platform — with identity propagation, durable execution, audit trails, cost attribution, and recovery logic — is closer to fifty lines [of code] per token, and most of those lines have nothing to do with the model."

**Harness engineering convergence references:**
- Anthropic: "Effective Harnesses for Long-Running Agents" (Jan 2026)
- LangChain: "The Anatomy of an Agent Harness" (Mar 2026)
- Vercel: deleted 80% of tools → success rate jumped from 80% to 100%
- LangChain: moved from Top 30 to Top 5 on Terminal Bench 2.0 by changing only the harness
- Harvey's legal agents: more than doubled accuracy through harness optimization alone

**ServiceNow Enterprise AI Maturity Index (2025):** Global maturity scores dropped from 44 to 35 year-over-year on a 100-point scale; fewer than 1% of organizations scored above 50.
