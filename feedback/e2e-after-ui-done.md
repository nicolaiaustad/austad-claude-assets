---
name: Run e2e tests after all UI changes, not per-edit
description: Don't run e2e tests on every file edit -- run them once when all UI changes are complete
type: feedback
---

Run e2e tests only after finishing all UI edits, not after each individual Edit/Write.

**Why:** Running browser-based tests on every file save causes massive slowdowns during UI iteration -- Playwright tests can take up to 120s per run, blocking the workflow.

**How to apply:** After completing a batch of UI changes, run the relevant e2e tests once. Don't wire up PostToolUse hooks that trigger e2e on every .html/.css/.js edit.
