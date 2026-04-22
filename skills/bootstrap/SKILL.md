---
name: bootstrap
description: "Cold-start a new project with proven workflow rules and universal feedback patterns from austad-claude-assets. Use when starting a new project, entering a repo for the first time, or when the user says /bootstrap, 'set up this project', 'cold start', or 'init claude code'."
---

# Bootstrap -- Cold-Start a New Project

Set up a new project with the user's proven workflow rules and universal feedback
patterns from the austad-claude-assets harness repo.

Run all phases in order without asking for approval on each step. Present a
consolidated report at the end.

## Phase 1: Ensure harness repo exists

```bash
REPO_DIR="$HOME/austad-claude-assets"
if [ ! -d "$REPO_DIR/.git" ]; then
  git clone https://github.com/nicolaiaustad/austad-claude-assets.git "$REPO_DIR"
else
  cd "$REPO_DIR" && git pull --rebase origin main
fi
```

## Phase 2: Set up project CLAUDE.md

If no `CLAUDE.md` exists in the current working directory, copy the global one:

```bash
if [ ! -f "CLAUDE.md" ]; then
  cp "$REPO_DIR/global/CLAUDE.md" ./CLAUDE.md
fi
```

If a `CLAUDE.md` already exists, skip this step and report that it was skipped.

## Phase 3: Set up project memory with universal feedback

Determine the project memory directory from the current working directory.
Claude Code sanitizes both `/` and `_` to `-` when deriving the memory dir name,
so the sed expression must replace both:

```bash
PROJECT_PATH=$(pwd | sed 's|[/_]|-|g')
MEMORY_DIR="$HOME/.claude/projects/$PROJECT_PATH/memory"
mkdir -p "$MEMORY_DIR"
```

Copy universal feedback patterns that don't already exist in the project memory:

```bash
for f in "$REPO_DIR/feedback/"*.md; do
  filename=$(basename "$f")
  if [ ! -f "$MEMORY_DIR/$filename" ]; then
    cp "$f" "$MEMORY_DIR/$filename"
  fi
done
```

If a `MEMORY.md` index doesn't exist, create a minimal one with links to the
copied feedback files:

```bash
if [ ! -f "$MEMORY_DIR/MEMORY.md" ]; then
  # Create MEMORY.md with a Feedback section listing copied files
fi
```

Generate the MEMORY.md content by listing all feedback files that were copied,
using the format:
```markdown
# Project Memory

## Feedback
- [bug-reports-include-fix](bug-reports-include-fix.md) -- Fix bugs alongside tests
- [no-external-api-for-generation](no-external-api-for-generation.md) -- Use subagents, not API scripts
...
```

## Phase 4: Recommend plugins

Check what kind of project this is by looking at files in the current directory:

| File present | Recommended plugin |
|---|---|
| `package.json` or `*.tsx`/`*.jsx` | `ui-ux-pro-max` skill for frontend work |
| `*.py` or `pyproject.toml` | `ruff` for linting |
| `.github/` directory | `github` plugin for PR management |

List recommendations with install commands but do NOT auto-install.

## Report

Present a summary:

```
Project: <current directory name>
CLAUDE.md: <created / already existed>
Feedback patterns: <count> copied to memory
Plugins recommended: <list or "none">
```
