# /learn agents cannot write files — Freyr must write

**Date**: 2026-03-20
**Source**: Session 3, /learn runs on gemgen, PhoneDriver, VEO3-Extention

## The Pattern

When `/learn` spawns Haiku subagents in **background mode**, those agents are **read-only**. They can read source code and return text output, but they cannot write files to `ψ/learn/`.

**Fixed workflow:**
1. Spawn all 3 Haiku agents in background with `run_in_background: true`
2. Wait for all to complete
3. Freyr (main agent) writes the files from the agents' returned text

This is the correct pattern — not a workaround. It applies every time `/learn` is run.

## Why This Matters

If you try to have agents write files directly (as the `/learn` skill instructions imply), the files will never appear. The agents return their output as text. Freyr must intercept that output and call Write() tool to persist it.

## Signal to Act On

If you see yourself re-learning this at the start of a /learn run, that's a sign this lesson hasn't been internalized yet. Remember: **background agents = read-only**.

## Bonus Pattern: Workflow > Feature Docs

When writing strategy plans or GitHub Issues for automation tools (GemLogin, GemPhoneFarm, etc.):
- **Start from actual workflow files** (`.gemlogin`, `.GemPhoneFarm` structures)
- **Not from feature documentation** (what the tool claims to do)
- The real use case is always more specific than the docs suggest

Ask before writing: "Is this plan tool-centric or workflow-centric?"
