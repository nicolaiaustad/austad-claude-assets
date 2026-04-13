---
name: No external API calls for generation tasks
description: Use Claude Code's own capabilities (subagents, Write tool) for content generation instead of scripts that call the Anthropic API with the user's API key
type: feedback
---

Don't write scripts that call the Anthropic API for content generation tasks that Claude Code can do itself (e.g., generating YAML config files, demo data, seed content).

**Why:** The user pays for Claude Code already -- using a separate API script burns their API credits unnecessarily for work Claude Code can handle directly via subagents and the Write tool.

**How to apply:** When generating bulk content (configs, test data, seed files), use parallel subagents to write the files directly instead of creating a Python script that calls `anthropic.Anthropic()`.
