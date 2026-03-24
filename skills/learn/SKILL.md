---
name: learn
description: "Back up all global Claude Code customizations (CLAUDE.md, settings, skills, memory, plugins) to the austad-claude-assets GitHub repo. Also reviews the current session for corrections and insights to save as memory before syncing. Use when the user says /learn, 'back up my config', 'sync my settings', 'save my customizations', or 'push my learnings'."
---

# Learn -- Claude Code Configuration Backup

Back up all global Claude Code customizations to https://github.com/nicolaiaustad/austad-claude-assets.git

Run three phases in order. All phases auto-apply without asking for approval on
each step. Present a consolidated report at the end.

## Phase 1: Capture Session Learnings

Review the current conversation for actionable learnings:

- **Corrections** the user gave (e.g., "don't do X", "use Y instead")
- **New patterns** or gotchas discovered during the session
- **User preferences** expressed (coding style, tool preferences, workflow choices)
- **Project knowledge** learned (architecture decisions, deployment details, team context)

For each learning, decide the right destination:

| Type | Destination |
|---|---|
| Global rule or preference | Edit `~/.claude/CLAUDE.md` |
| Project-specific insight | Save to `~/.claude/projects/<current-project>/memory/` using standard memory frontmatter |
| Debugging pattern or gotcha | Save as memory file with type `feedback` |
| User role or context | Save as memory file with type `user` |

**Memory file format** (when creating new memory files):
```markdown
---
name: descriptive-name
description: one-line description used for relevance matching
type: feedback|user|project|reference
---

The learning content. For feedback/project types, structure as:
Rule or fact statement.
**Why:** the reason or incident behind it.
**How to apply:** when and where this guidance kicks in.
```

Save all learnings before proceeding. If no learnings were found in the session,
say "No new learnings to capture" and move to Phase 2.

## Phase 2: Sync Global Assets to Repo

### Step 1: Ensure repo exists

```bash
REPO_DIR="$HOME/austad-claude-assets"
if [ ! -d "$REPO_DIR/.git" ]; then
  git clone https://github.com/nicolaiaustad/austad-claude-assets.git "$REPO_DIR"
fi
```

### Step 2: Pull latest (skip on empty repo)

```bash
cd "$REPO_DIR"
git log --oneline -1 2>/dev/null && git pull --rebase origin main || true
```

### Step 3: Sync global config files

```bash
mkdir -p "$REPO_DIR/global/plugins"

cp "$HOME/.claude/CLAUDE.md" "$REPO_DIR/global/CLAUDE.md" 2>/dev/null || true
cp "$HOME/.claude/settings.json" "$REPO_DIR/global/settings.json" 2>/dev/null || true
cp "$HOME/.claude/settings.local.json" "$REPO_DIR/global/settings.local.json" 2>/dev/null || true

for f in installed_plugins.json known_marketplaces.json blocklist.json; do
  cp "$HOME/.claude/plugins/$f" "$REPO_DIR/global/plugins/$f" 2>/dev/null || true
done
```

### Step 4: Sync global skills

For each directory in `~/.claude/skills/*/`, copy the entire directory
(preserving structure) into `$REPO_DIR/skills/<skill-name>/`.

```bash
if [ -d "$HOME/.claude/skills" ]; then
  mkdir -p "$REPO_DIR/skills"
  for skill_dir in "$HOME/.claude/skills"/*/; do
    skill_name=$(basename "$skill_dir")
    rm -rf "$REPO_DIR/skills/$skill_name"
    cp -r "$skill_dir" "$REPO_DIR/skills/$skill_name"
  done
fi
```

### Step 5: Sync memory files

For each project directory in `~/.claude/projects/*/` that has a `memory/`
subdirectory, copy all `.md` files into `$REPO_DIR/memory/<project-name>/`.

Derive readable project names by stripping the encoded path prefix:
- `-Users-nicolaiaustad-Local-Code-realm` becomes `realm`
- `-Users-nicolaiaustad-conductor-workspaces-realm-philadelphia` becomes `realm-philadelphia`
- `-Users-nicolaiaustad-Library-CloudStorage-OneDrive-Personal-Programming-Guru` becomes `guru`

Logic: for conductor workspaces, take the last two segments joined with `-`.
For everything else, take the last segment.

```bash
for proj_dir in "$HOME/.claude/projects"/*/; do
  mem_dir="$proj_dir/memory"
  if [ -d "$mem_dir" ] && ls "$mem_dir"/*.md 1>/dev/null 2>&1; then
    encoded=$(basename "$proj_dir")

    if echo "$encoded" | grep -q "conductor-workspaces"; then
      project_name=$(echo "$encoded" | sed 's/.*conductor-workspaces-//' | sed 's/-/-/' )
    else
      project_name=$(echo "$encoded" | rev | cut -d'-' -f1 | rev)
      if [ ${#project_name} -le 2 ]; then
        project_name=$(echo "$encoded" | rev | cut -d'-' -f1,2 | rev)
      fi
    fi

    mkdir -p "$REPO_DIR/memory/$project_name"
    cp "$mem_dir"/*.md "$REPO_DIR/memory/$project_name/"
  fi
done
```

### Step 6: Generate README.md

Create or update `$REPO_DIR/README.md` with:
- Title: "Nicolai Austad -- Claude Code Assets"
- Description: what this repo contains and why
- Last synced timestamp
- Directory listing of what is backed up (use `find` or `tree` output)
- Note that this repo is auto-maintained by the `/learn` Claude Code skill

## Phase 3: Commit & Push

```bash
cd "$REPO_DIR"
git add -A
```

Run `git diff --cached --stat` and present the summary to the user.

If there are staged changes:

```bash
git commit -m "Sync Claude Code assets -- $(date +%Y-%m-%d)"
git push origin main
```

If there are no changes, report "Everything is already up to date" and skip
commit/push.

## Report

After all phases complete, present a summary:

```
Session learnings: <count> saved
Files synced: <count from git diff stat>
Commit: <hash> pushed to origin/main
```

If any phase had issues, flag them clearly.
