# Stage 1 - Foundations

## Outcome
- **Task 1.1, set the site in Literata:** Success. The User approved it on localhost and it was merged into `main` by fast-forward, at `8cd9b3b`. Worker: site-agent, conversation 1. Log: [Original](task-01-01.log.md).
- **Task 1.2, Animations switch and saved mode:** Partial in its follow-up. The User accepted that outcome on localhost, with the limit listed under Validation and Limits, and it was merged into `main` at `cc00cfc`. Worker: site-agent, conversation 1. Logs: [Original](task-01-02.log.md) and [Follow-up 1](task-01-02.followup-01.log.md), the final reviewed attempt.
- **Commits on `main`, in order:** `8cd9b3b` Literata; `3c2adcf` switch and saved mode; `063c6bf` and `46658a5` review fixes for the On/Off word; `728c436` moves the switch to a footer; `c384ae6` navbar uses the page background; `43e9a8e` removes the navbar shadow; `cc00cfc` fixes the switch at the bottom right.
- **Workers:** site-agent only. Nothing ran in parallel.
- **Branches:** `feat/literata-font` and `feat/animations-switch` are merged and kept. Deleting them is the User's decision.
- **Stage 1 is complete.** Stage 2 has not started. The session pauses before it, as the User decided.

## Findings and Decisions
- **Switch placement.** The User moved the Animations switch from the navbar to a footer at the right. Then the User asked for it to be fixed at the bottom right, so it stays in view. The Spec, Plan and Tracker record both. Source: [follow-up log](task-01-02.followup-01.log.md), "Later feedback from the User".
- **Navbar.** At the User's request, the navbar uses the page background colour, and its shadow is removed. Source: [follow-up log](task-01-02.followup-01.log.md).
- **Navbar spacing.** The original Task 1.2 log found that the navbar spread its seven items and moved the links. Moving the switch out of the navbar resolved it. Source: [original log](task-01-02.log.md), Important Findings 1.
- **20-pixel difference after toggling at 390px.** The original log found it, without a cause. The follow-up re-test found no changed pixels in the navbar region, because the switch no longer sits there. Source: [follow-up log](task-01-02.followup-01.log.md), item 9.
- **Same-look check.** The follow-up found 10 pixels outside the switch at light 390px, each changed by one level, in navbar link text. The User accepted the Partial outcome. The basis is that a repeat of the same comparison within one mode differed far more. Source: [follow-up log](task-01-02.followup-01.log.md), Important Finding 1.
- **Inputs for Task 2.3.** The switch's background and track fade with the theme in animated mode, and change at once in minimal mode. The hover effect is not decided. The User decides it before the Task 2.3 prompt is written. Source: [follow-up log](task-01-02.followup-01.log.md), "Note for Task 2.3".
- **Spec and Plan.** The switch's placement and the navbar change are recorded in the Spec's Overview, Motion Modes and Notes, and in Plan Tasks 1.2 and 2.3.
- **Deferred to a later session.** The content restructure, the copy and the wider redesign, as the User decided.

## Validation and Limits
Stage verification, run by the Manager on `main` at `cc00cfc`:
- `tools/contrast_check.py style.css`: 16 of 16 pairs pass.
- `tools/theme_check.mjs`: 20 of 20 checks pass. The no-preference case is skipped, because headless Chromium reports light for no-preference. This is a known limit.
- `tools/focus_check.mjs`: 8 of 8 pass. It covers the theme toggle. The switch's focus ring is checked in the Task 1.2 logs.
- `tools/qa_matrix.mjs`: 15 pass, 0 fail, and 2 skipped for the same no-preference limit. It also gives the phone line-length note below.
- Manager check on the merged branch, at 1280px and 390px, in light and dark: 38 of 38 pass. It covers the mode from the system and from saved choices in both directions, persistence across reloads, the switch in the footer and in view at the bottom of the page, body text in Literata at 400, headings and strong at 500, no horizontal scroll, no console errors, click and Space, the theme toggle working independently of motion, and the switch hidden without JavaScript. The check script is in the scratchpad and is not kept.
- Not run: `tools/motion_check.mjs`. It belongs to Task 3.1 in this session's Stage 3.

Accepted limits:
- **Same-look item at light 390px:** 10 pixels outside the switch, each changed by one level. Accepted by the User as a Partial outcome.
- **Phone line length:** 27 characters per line at 390px with Literata. Session 1 measured 30 with IBM Plex Sans and accepted it as a phone limit. The desktop value is 55, in range. Sources: this stage's QA matrix run, and [session 1 Stage 3 summary](../../archives/session-2026-11-15-001/memory/stage-03/stage-03.summary.md).
- **Hover effect on the switch:** not decided. Needed before Task 2.3.

Worker check scripts and screenshots are in `~/.cache/site-agent-checks/`, outside the repository.

## Task Logs

- Task 1.1: [Original](task-01-01.log.md)
- Task 1.2: [Original](task-01-02.log.md), [Follow-up 1](task-01-02.followup-01.log.md), the final reviewed attempt
