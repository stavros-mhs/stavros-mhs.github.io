# Stage 3 - QA

## Outcome
- **Task 3.1, QA matrix and final checks:** Success, accepted by the Manager. No repository change, so no merge. The branch `test/theme-qa` had no commits and is deleted. Worker: `site-agent`, conversation 1. The User confirmed the look at desktop width and on a narrow window.
- **Remaining from this Stage:** none. The phone line length is accepted as a limit (see below). The favicon is an idea for later.

## Findings and Decisions
- **Phone line length is 30 characters per line at 390px,** below the 45 to 90 target. The desktop value of 55 is in range. The Manager accepts this as a phone limit. Reaching 45 would need a text size too small to read comfortably, and the Plan's target stays as written for desktop. Source: [3.1 log](task-03-01.log.md).
- **Two matrix cells cannot run in this Chromium** ("No preference", at both widths). They are covered by the Node simulation accepted in Stage 1. Source: [3.1 log](task-03-01.log.md), [1.3 log](../stage-01/task-01-03.log.md).

## Validation and Limits
- **Stage verification (the Task 3.1 QA pass, on the combined `main` at `578b282`):**
  - Contrast script: 16 of 16 pairs PASS.
  - QA matrix at 1280px and 390px: 8 of 8 runnable cells PASS. Two "No preference" cells SKIP.
  - Phone layout at 390px: no horizontal scroll, the toggle is visible in the navbar in both themes, and the h1 fits its container.
  - Keyboard focus check: 8 of 8 PASS.
  - Line length: 55 characters per line at 1280px. 30 characters per line at 390px, accepted as a limit.
  - Screenshots are in the scratchpad: `qa-shots/`.
- **Limits:**
  - The "No preference" cells are untested in a browser, as accepted in Stage 1.
  - Phone line length is below the target, as accepted above.
  - The favicon 404 is open, and recorded as an idea for later.

## Task Logs
- Task 3.1: [Original](task-03-01.log.md)
