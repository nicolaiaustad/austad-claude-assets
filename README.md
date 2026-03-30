# Nicolai Austad -- Claude Code Assets

Generic Claude Code starter setup. Auto-maintained by the `/learn` skill.

**Last synced:** 2026-03-31

## What is this?

This repo stores my global Claude Code customizations so they are never lost.
It serves as a clean starter kit for any new project -- no project-specific
content, only universally applicable configurations.

## Contents

- **global/** -- Global CLAUDE.md, settings, and plugin configurations
- **skills/** -- Custom global skills available across all projects
- **toolbox.md** -- Tools and plugins used across projects (plugins auto-detected, dev tools manually curated)

## Directory Structure

```
global/
  CLAUDE.md
  settings.json
  settings.local.json
  plugins/
    blocklist.json
    installed_plugins.json
    known_marketplaces.json
skills/
  interview-me/       Interview skill
  learn/              This backup skill
toolbox.md
```

## Usage

Invoke `/learn` in any Claude Code session to:
1. Review the session for new corrections/insights and save them as memory
2. Sync all global assets to this repo
3. Auto-generate the toolbox from installed plugins
4. Commit and push
