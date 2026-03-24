---
name: learn-skill-scope
description: The /learn skill should only sync global ~/.claude/ assets, not project-specific .claude/ directories
type: feedback
---

The /learn skill backs up only global Claude Code assets to the austad-claude-assets GitHub repo. It should NOT scan project directories for configs or include project-specific .claude/ content.
**Why:** Project configs are checked into their own repos. The GitHub backup repo is specifically for global, cross-project assets that would otherwise be lost.
**How to apply:** When updating or extending the /learn skill, keep the scope to ~/.claude/CLAUDE.md, settings, plugins, global skills, and project memory files only.
