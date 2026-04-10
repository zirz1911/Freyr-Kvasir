---
date: 2026-04-02
source: rrr: Thanjai-phone
tags: [background-agents, gh-cli, issue-editing]
---

# Background Agents Need Explicit Tool Grants at Spawn Time

**Pattern:** When spawning a background agent to run shell commands (e.g. `gh issue edit`), Bash tool permission must be granted at launch. If not, the agent completes all cognitive work but cannot execute — it hands back a "here's what to run" message instead of a completed action.

**Why it matters:** From the user's perspective, the task notification says "completed" but the actual side effect (issue updated) never happened. The main agent then has to re-read the context and redo the work. Net effect: added latency, no saved effort.

**How to apply:**
- Before spawning a background agent for `gh` commands or git operations, verify it has Bash in its tool access
- If unsure, either handle it in the main agent directly, or accept that the agent will return a script to run rather than executing it
- For purely read/research tasks, background agents work fine without Bash
