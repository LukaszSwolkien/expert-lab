# Engineering leader: templates vs local data

Files in this directory are **committable templates** (structure and section prompts). They stay generic so the repo can be shared on GitHub without leaking personal or employer-specific context.

**Your filled-in files** belong in **`.engineering-leader/`** at the **repository root** (sibling to `AGENTS.md`). That directory is listed in `.gitignore` and is not tracked by git.

## One-time setup

From the repository root:

```sh
mkdir -p .engineering-leader
cp personas/engineering-leader/profile.md .engineering-leader/profile.md
cp personas/engineering-leader/goals.md .engineering-leader/goals.md
cp personas/engineering-leader/engineering-leader-context.md .engineering-leader/engineering-leader-context.md
```

Edit the copies under `.engineering-leader/`. The `engineering-leader` persona loads those paths first; see `personas/engineering-leader.md` for rules.

**Scope:** This template-vs-local split is defined for `engineering-leader` only. Other personas in `personas/` do not use `.engineering-leader/` unless their own persona file is updated to say so.
