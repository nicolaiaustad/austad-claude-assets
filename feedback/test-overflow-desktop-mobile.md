---
name: test-overflow-desktop-mobile
description: Always test UI changes for text overflow on both desktop and mobile viewports
type: feedback
---

All UI changes must be tested for text overflow on both desktop and mobile viewports.

**Why:** Text that fits on desktop frequently overflows on mobile, causing broken layouts. This has been a recurring issue.

**How to apply:** When making any text/layout changes to a frontend, verify the content doesn't overflow on narrow screens (~320-375px width) as well as desktop. Use `overflow-wrap: break-word`, `word-break`, or responsive text sizing as needed.
