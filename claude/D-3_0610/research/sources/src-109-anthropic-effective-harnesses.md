---
URL: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
Title: Effective harnesses for long-running agents
AccessDate: 2026-06-10
Publisher: Anthropic Engineering Blog
Published: November 26, 2025
Author: Justin Young (Anthropic)
Related section: Harness design, prompt as structured configuration, long-running agents, initializer vs. coding agent pattern
---

## Key Excerpt

### The Long-Running Agent Problem
> "The core challenge of long-running agents is that they must work in discrete sessions, and each new session begins with no memory of what came before."
>
> "Getting agents to make consistent progress across multiple context windows remains an open problem."

The analogy used: "a software project staffed by engineers working in shifts, where each new engineer arrives with no memory of what happened on the previous shift."

### Two-Part Solution:
1. **Initializer agent**: Sets up environment on the first run; creates `init.sh`, `claude-progress.txt` (log of what agents have done), initial git commit showing files added
2. **Coding agent**: Makes incremental progress in every session, then leaves clear artifacts for the next session

### Key Harness Design Elements:

**Feature list file (structured JSON, not Markdown):**
```json
{
  "category": "functional",
  "description": "New chat button creates a fresh conversation",
  "steps": ["Navigate to main interface", "Click the 'New Chat' button", ...],
  "passes": false
}
```
The model is instructed "It is unacceptable to remove or edit tests." JSON chosen over Markdown as model is less likely to inappropriately overwrite JSON files.

**Incremental progress with git:**
Coding agents commit progress to git with descriptive commit messages and write summaries in progress file. This allows rollback to recover working states.

**Initialization sequence for each coding agent:**
1. Run `pwd` to see directory
2. Read git logs and progress files to get up to speed
3. Read the features list file and choose the highest-priority feature not yet done

### Agent Failure Modes and Solutions:
| Problem | Initializer Agent Behavior | Coding Agent Behavior |
|---------|---------------------------|----------------------|
| Claude declares victory too early | Set up feature list file with structured tests | Read feature list; choose one feature to work on |
| Claude leaves buggy/undocumented state | Set up git repo and progress notes file | Start by reading progress notes and git logs; end with git commit |
| Claude marks features done prematurely | Set up feature list file | Self-verify features; only mark "passing" after careful testing |
| Claude can't figure out how to run app | Write `init.sh` script | Start by reading `init.sh` |

### Key Insight (Harness as encoded procedure):
> "The key insight here was finding a way for agents to quickly understand the state of work when starting with a fresh context window, which is accomplished with the claude-progress.txt file alongside the git history. Inspiration for these practices came from knowing what effective software engineers do every day."

This exemplifies **harness as asset-ized human procedure** — the progress file, feature list, init script, and git discipline are exactly the kind of "standard operating procedure" that makes human engineering teams effective, now encoded into the harness for the agent.

### Reference to Supporting Docs:
- "Effective Harnesses for Long-Running Agents" (Nov 2025)
- "Harness Design for Long-Running Application Development" (Mar 2026): https://www.anthropic.com/engineering/harness-design-long-running-apps
