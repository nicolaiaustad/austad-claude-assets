# Nicolai Austad -- Claude Code Assets

Global Claude Code configuration backup. Auto-maintained by the `/learn` skill.

**Last synced:** 2026-03-24 19:26:52

## What is this?

This repo stores my global Claude Code customizations so they are never lost:
- Global CLAUDE.md (instructions that apply across all projects)
- Settings and plugin configurations
- Custom global skills
- Project memory files (learnings Claude accumulates per-project)

## Directory Structure

```
global
global/CLAUDE.md
global/plugins
global/plugins/blocklist.json
global/plugins/installed_plugins.json
global/plugins/known_marketplaces.json
global/settings.json
global/settings.local.json
memory
memory/realm
memory/realm-philadelphia
memory/realm-philadelphia/MEMORY.md
memory/realm-philadelphia/feedback_learn_skill_scope.md
memory/realm-philadelphia/reference_austad_claude_assets.md
memory/realm/MEMORY.md
skills
skills/interview-me
skills/interview-me/SKILL.md
skills/learn
skills/learn/SKILL.md
```

## Usage

Invoke `/learn` in any Claude Code session to:
1. Review the session for new corrections/insights and save them as memory
2. Sync all global assets to this repo
3. Commit and push
