---
name: /learn Skill — Real-World Constraints
description: Practical limitations discovered when running /learn in Freyr's environment
type: feedback
---

## Rule: ghq not installed — use git clone fallback

When `/learn` runs and `ghq` is not available, use:
```bash
git clone --depth=1 "https://github.com/OWNER/REPO" "$HOME/ghq/github.com/OWNER/REPO"
```
Then create the symlink manually as the skill instructs.

**Why:** `ghq` is not installed on this machine. The skill's Step 0 assumes it. Exit code 127 means command not found.

**How to apply:** Always check for ghq first: `which ghq`. If missing, use the git clone fallback path.

---

## Rule: Haiku subagents are read-only — write files yourself

When spawning subagents for `/learn`, they will explore and return content but cannot write files.

**Why:** Subagent permission model in this environment restricts write access.

**How to apply:** After agent returns, take their output and use the `Write` tool to create the documentation files. The agent does the research; Freyr does the writing.

---

## Rule: ψ/learn/ is gitignored — decide before committing

`ψ/.gitignore` currently contains `learn/` which ignores ALL learn documentation.

**Why:** Default Kvasir setup gitignores learn content.

**How to apply:** Before assuming learn docs will be committed, check `ψ/.gitignore`. If wanting to commit docs but ignore symlinks, change `learn/` to `learn/**/origin`.
