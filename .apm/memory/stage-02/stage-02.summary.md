# Stage 2 - Terminal landing (desktop)

## Outcome
Tasks 2.1 to 2.7 are Done, all on `site-agent`. 2.1 to 2.4 are merged into `main`. 2.5, 2.6 and 2.7 (with follow-up 01) are on `feat/md-transition`, fast-forwarded into `dev` at `a2a5642` on 23 Nov, after the User's localhost approval. Task 2.3 was covered by 2.2. `main` itself holds only the work in progress page (`4aa93ae`), and `dev` merges into it when the site is finished.

## Findings and Decisions
- **The site is a terminal.** `whoami` and `tree` type on the landing, the navbar drops, folders open in place, and each `.md` file opens as a page with the markdown below the terminal (Tasks 2.1, 2.2, 2.5).
- **Cd model (`f951538`, no Task Prompt; kept by the User on 23 Nov).** Moving to another folder types a `cd` first. The Spec's Terminal Landing carries the details. Source: Handoff Log `handoff-02.log.md`.
- **Prompt colour.** `--prompt` is `#edae77` dark and `#934e25` light, at 4.61:1 on the surface. The dark prompt is nearly the same as the directory colour, and the User left it.
- **Contrast.** Light directory names on the terminal keep 3.31:1 (agreed exception). In light mode, the footnote and navbar and panel names below 24px use `--dir-hover` (5.64:1). The 24px light names stay at 3.82:1, as large text (Task 2.7, User decision).
- **Switch on phones** sits in the page flow at the bottom instead of fixed (Task 2.7). Desktop is unchanged.
- **Prompt output is instant (Task 2.7 follow-up 01).** The prompt path appears at once, and only commands type at human speed.
- **Lost machine.** `tools/` was rebuilt in Task 2.6, on this machine, from descriptions. Counts from earlier conversations do not carry over.
- **Open for later.** Phone text wrapping (User dislikes it, Task 4.2). The minimal-mode link transition goes in Task 3.3. Light-mode small links change only by underline on hover (Task 3.2 should know). `qa_matrix.mjs` is not a gate until Task 4.3.

## Validation and Limits
Stage verification ran on the final head `a2a5642`, through Tasks 2.6 and 2.7: contrast 52 pairs, 0 failed; `phone-check` 1064/0 at 360 and 390px; `pages-test` 288/0; `md-test` 79/0 (2 skipped); `footnote-check` 180/0; `navbar-root-check` 210/0; `wrap-verify` 268/0; `switch-check` 60/0; `colour-check` 26/0; `prompt-timing` passing. Limits: the checks are new code, the phone matrix runs in minimal mode, and `prefers-reduced-motion: reduce`, real phones and screen readers were not tested. Evidence: the Task Logs below.

## Task Logs

- Task 2.1: [Original](task-02-01.log.md), [Follow-up 1](task-02-01.followup-01.log.md), [2](task-02-01.followup-02.log.md), [3](task-02-01.followup-03.log.md), [4](task-02-01.followup-04.log.md), [5](task-02-01.followup-05.log.md)
- Task 2.2: [Original](task-02-02.log.md)
- Task 2.4: [Original](task-02-04.log.md)
- Task 2.5: [Original](task-02-05.log.md)
- Task 2.6: [Original](task-02-06.log.md)
- Task 2.7: [Original](task-02-07.log.md), [Follow-up 1](task-02-07.followup-01.log.md)
