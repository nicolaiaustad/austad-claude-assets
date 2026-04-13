# Nicolai Austad -- Claude Code Assets

Cold-start harness for new projects. Auto-maintained by `/learn`, applied by `/bootstrap`.

**Last synced:** 2026-04-13

## What is this?

This repo stores my global Claude Code configuration and proven workflow
patterns. It serves two purposes:

1. **Cold-start new projects** -- Run `/bootstrap` in any repo to get my
   workflow rules, feedback patterns, and plugin recommendations instantly.
2. **Back up and evolve** -- Run `/learn` to capture session learnings and sync
   global config. Universal feedback patterns propagate to all future projects.

## Contents

```
global/          Global CLAUDE.md, settings, plugin configs
feedback/        Universal feedback patterns (copied to new projects by /bootstrap)
skills/
  bootstrap/     Cold-start a new project
  learn/         Back up config and capture session learnings
  interview-me/  Stress-test a plan or design
toolbox.md       Plugins and dev tools inventory
```

## Quick start

**New project:** Run `/bootstrap` in the project directory. This copies the
global CLAUDE.md as the project CLAUDE.md, seeds your memory with universal
feedback patterns, and recommends plugins.

**End of session:** Run `/learn` to capture corrections and insights, sync
global config, and push changes here.

## How feedback flows

```
You correct Claude in project A
        |
  /learn saves it to feedback/
        |
  /bootstrap copies it to project B
        |
Claude avoids the same mistake in project B
```

Universal feedback (applies to any project) lives in `feedback/`. Project-specific
feedback stays in that project's memory only.
