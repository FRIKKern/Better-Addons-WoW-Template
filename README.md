# WoW Addon Template — Midnight 12.0+

> Clone. Rename. Code. Tag. Ship. A production-ready WoW addon starter with CI/CD that packages and uploads to CurseForge, Wago, and WoWInterface automatically.

## Quickstart

```bash
# 1. Clone the template
git clone https://github.com/FRIKKern/WoW-Addon-Template.git MyAddonName
cd MyAddonName

# 2. Rename everything (requires Claude Code + better-addons plugin)
#    This renames files, globals, TOC metadata, and all internal references.
/wow-setup MyAddonName "Your Name" "Short description of your addon"

# 3. Code your addon
#    Edit Init.lua, Core.lua, Config.lua — see CLAUDE.md for patterns

# 4. Test locally
ln -s "$(pwd)" "/path/to/WoW/_retail_/Interface/AddOns/MyAddonName"
# /reload in-game

# 5. Ship it
git tag v1.0.0 && git push origin v1.0.0
# GitHub Actions handles the rest
```

## What's Included

```
MyAddon.toc              # Addon metadata — Interface 120001 (Midnight)
Init.lua                 # Namespace, events, SavedVariables, slash commands
Core.lua                 # Feature logic, combat queue, hooks, frame creation
Config.lua               # Settings API panel (checkboxes, sliders, dropdowns)
CLAUDE.md                # AI instructions — deprecated APIs, Secret Values, patterns
.cursorrules             # Cursor IDE instructions (same rules, compact format)
Libs/embeds.xml          # Library loader (LibStub, CallbackHandler)
.pkgmeta                 # BigWigsMods/packager config — externals, ignores, nolib
.github/workflows/
  release.yml            # Tag push → lint → package → upload to 4 platforms
  lint.yml               # Luacheck on every push to main and every PR
.luacheckrc              # WoW-aware Luacheck config with all globals declared
.luarc.json              # Lua Language Server config (Lua 5.1, WoW builtins disabled)
.vscode/                 # VS Code settings + recommended extensions
.editorconfig            # Consistent formatting across editors
LICENSE                  # MIT
CHANGELOG.md             # Keep a Changelog format
```

## CI/CD Pipeline

This is the core value proposition. Push a tag, get a release on four platforms.

### Release Flow (`release.yml`)

```
git tag v1.0.0 → git push origin v1.0.0
                         │
                    GitHub Actions
                         │
              ┌──────────┴──────────┐
              │                     │
          Luacheck              Package
        (lint gate)          BigWigsMods/packager@v2
              │                     │
              │              ┌──────┼──────┬──────────┐
              │              │      │      │          │
              │          CurseForge Wago  WoWI  GitHub Release
              │              │      │      │          │
              └──────────────┴──────┴──────┴──────────┘
```

The packager automatically:
- Replaces `@project-version@` tokens with the tag name
- Fetches libraries from `.pkgmeta` externals
- Generates a changelog from git history
- Creates both full and `-nolib` zip packages
- Uploads to every platform where a secret is configured

### Lint Flow (`lint.yml`)

Runs Luacheck on every push to `main` and every pull request. The `.luacheckrc` declares all WoW globals so you get real signal, not noise.

### Required Secrets

Add these in your GitHub repo under **Settings > Secrets and variables > Actions**:

| Secret | Platform | Where to Get It |
|--------|----------|-----------------|
| `CF_API_KEY` | CurseForge | [Author Dashboard](https://authors.curseforge.com) > API Tokens |
| `WAGO_API_TOKEN` | Wago Addons | [Account > API Keys](https://addons.wago.io/account/apikeys) |
| `WOWI_API_TOKEN` | WoWInterface | File Management > API Token |

GitHub Releases uses the built-in `GITHUB_TOKEN` — no setup needed.
Missing a secret? That platform is simply skipped. Start with just GitHub Releases (zero config), add others later.

**Also required:** Go to **Settings > Actions > General > Workflow permissions** and select **Read and write permissions**.

### Platform IDs

Update these in `MyAddon.toc` with your real project IDs:

```
## X-Curse-Project-ID: 123456
## X-Wago-ID: abcdefgh
## X-WoWI-ID: 12345
```

## Claude Code Integration

This template is designed for AI-assisted development with [Claude Code](https://claude.ai/claude-code) and the [better-addons](https://github.com/FRIKKern/better-addons) plugin.

| Command | What It Does |
|---------|-------------|
| `/wow-setup` | One-command rename — files, globals, TOC, all references |
| `/wow-create` | Generate a complete addon from a description |
| `/wow-review` | Audit for deprecated APIs, taint risks, Secret Values issues |
| `/wow-api` | Look up any WoW API function or event |
| `/wow-debug` | Diagnose in-game errors and symptoms |
| `/wow-migrate` | Update pre-12.0 code to Midnight compatibility |

The `CLAUDE.md` file teaches AI assistants the Midnight 12.0+ rules: which APIs are removed, how Secret Values work, and what patterns to follow. It works with Claude Code, Cursor (via `.cursorrules`), and any LLM that reads project context.

## Template Features

**Midnight 12.0+ Ready** — Interface 120001, no deprecated APIs, Secret Values handled via `issecretvalue()` + `ns.SafeValue()` guard pattern.

**Pure Blizzard API** — No Ace3 dependency. Namespace pattern (`local addonName, ns = ...`), table-dispatch events, modern Settings API (11.0.2+ signatures).

**Addon Compartment** — Minimap dropdown entry with click/hover handlers, registered via TOC metadata.

**Combat Lockdown Queue** — Operations deferred during combat, flushed on `PLAYER_REGEN_ENABLED`.

**SavedVariables with Migration** — Default merging on load, schema versioning pattern for future DB restructuring.

**hooksecurefunc Examples** — The Midnight-safe way to modify Blizzard frames, with `IsForbidden()` guards.

## License

MIT — see [LICENSE](LICENSE).
