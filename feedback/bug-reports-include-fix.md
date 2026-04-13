---
name: bug-reports-include-fix
description: When user describes a bug and asks for a test, also fix the bug
type: feedback
---

When the user describes a bug and asks for a test, fix the bug too -- don't just write the test.
**Why:** User had to explicitly ask for the fix after the test was written. They expected both in one go.
**How to apply:** If the user describes broken behavior and requests a test, treat it as two tasks: write the test AND fix the underlying issue. Only skip the fix if the user explicitly says "just the test".
