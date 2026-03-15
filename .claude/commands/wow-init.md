---
description: "Initialize a WoW addon project directory for addon development"
---

Initialize a WoW addon project: $ARGUMENTS

You set up a WoW addon project directory to work with this template's Claude Code skills.

## What This Does

1. Creates `.claude/modes/active-mode.md` with the default mode ("enhancement-artist")
2. Creates a `CLAUDE.md` file with WoW 12.0+ addon development context (if one doesn't exist)
3. Displays a welcome message with available commands and current mode

## Process

### Step 1: Set Up Mode

Check if `.claude/modes/active-mode.md` exists in the project root.

- **If not:** Create the `.claude/modes/` directory and write `enhancement-artist` to `active-mode.md`
- **If yes:** Read it and note the current mode

If `$ARGUMENTS` contains a recognized mode name (`blizzard-faithful`, `boundary-pusher`, `enhancement-artist`, `performance-zealot`, or their short aliases `faithful`, `boundary`, `enhance`, `performance`), use that as the active mode instead of the default.

### Step 2: Set Up CLAUDE.md

Check if `CLAUDE.md` exists in the project root.

- **If not:** Create a basic `CLAUDE.md` with:

```markdown
# Project: {project name from $ARGUMENTS or directory name}

This is a WoW addon project targeting **Patch 12.0+ (Midnight)**.

## Conventions

- **Language:** Lua 5.1 (WoW embedded runtime)
- **Interface version:** 120001
- **Expansion:** Midnight (12.0)
- **Secret Values:** All combat-related numeric data (health, damage, healing) may be opaque — never do arithmetic on potentially secret values
- **Security model:** Use `hooksecurefunc()` for post-hooks, check `IsForbidden()` on hooked frames, defer secure frame changes out of combat with `InCombatLockdown()` checks

## Available Commands

- `/wow-create` — Create a complete addon from scratch
- `/wow-review` — Review addon code for bugs and compatibility
- `/wow-debug` — Debug addon issues
- `/wow-mode` — Switch development philosophy mode
- `/wow-migrate` — Migrate pre-12.0 code to Midnight
- `/wow-api` — Look up WoW API documentation
- `/wow-news` — Find latest WoW addon ecosystem news
- `/wow-research` — Research and verify WoW addon information
- `/wow-verify` — Verify addon code or API claims
- `/wow-init` — Re-run this initialization
```

- **If yes:** Report that it already exists and skip creation

### Step 3: Display Welcome Message

Output the following welcome message (substitute the actual mode name):

```
Template initialized!

Current mode: {mode-name}

Available commands:
  /wow-create   — Create a complete addon
  /wow-review   — Review addon code
  /wow-debug    — Debug addon issues
  /wow-mode     — Switch development philosophy
  /wow-migrate  — Migrate to Midnight 12.0
  /wow-api      — Look up WoW API docs
  /wow-news     — Latest addon ecosystem news
  /wow-research — Research & verify information
  /wow-verify   — Verify code or API claims

Change mode: /wow-mode faithful|boundary|enhance|performance
```
