# APM Tracker

## Notes

- **Scope latitude (User):** Workers may add or change files in the repository where a Task needs them, and choose the best approach. Task Prompts name the files involved. Recorded in the Spec's Notes.
- **Corrections applied before dispatch:**
  - Task 1.1's commit message is now lowercase, as the Rules require.
  - Task 2.2 adds a visible keyboard focus ring on the theme toggle in `--focus`. The toggle has no focus style today, and the Spec names it.
  - Task 2.2 must also make the toggle focusable. Its checkbox was `display: none`, which removed it from the tab order, so the ring could never show. The Task Prompt made it visually hidden instead.
- **Stage 1 complete (Manager):** Tasks 1.1, 1.2 and 1.3 were each accepted as Partial and merged into `main`: `0e5b65a`, `4a4e32f`, `3d2d52a`. Summary: `.apm/memory/stage-01/stage-01.summary.md`.
- **Stage 2 complete (Manager):** Tasks 2.1, 2.2 and 2.3 were each accepted as Success and merged into `main`: `715aa05`, `570208a`, `578b282`. Summary: `.apm/memory/stage-02/stage-02.summary.md`.
- **Stage 3 complete (Manager):** Task 3.1, the QA pass, was accepted as Success. It made no repository change. Phone line length is 30 characters per line at 390px, accepted as a limit. Summary: `.apm/memory/stage-03/stage-03.summary.md`.
- **Task 1.1 remaining item:** the console 404 for `/favicon.ico`. The User skipped it. Idea for later, not a Task.
- **Task 1.2 deviations, accepted by the User:** dark `--link` changed to `#d8b78e`. Dark `--link-hover` is `#f5b06e`, not the Spec's `#f0a862`. Sun and moon icons changed to `--accent`. Visited links use `--link`, and active links use `--link-hover`. The Spec's palette table was corrected to match.
- **Task 1.3 remaining item:** browser Case 3 (no saved choice, system no preference) is accepted on the Node simulation, because headless Chromium cannot emulate that setting.
- **Task 2.2 decisions beyond the Plan, all approved by the User and recorded in the Spec:** section headings use the accent, the light palette is cream paper, headings and bold text use weight 500, the toggle shows the mode you would switch to, and the new tokens (`--focus`, `--knob-dark`, `--knob-light`, `--moon-icon`, `--sun-icon`).
- **Tools (User):** the test tooling now lives in `tools/`, ignored by Git, and is documented in `tools/README.md`. The `.gitignore` entry is committed as `ca642de` on `chore/ignore-tools`, which awaits merging. Adding the tools to the Rules is still pending the User's decision.
- **Idea for later (User):** add a favicon, which removes the 404 for `/favicon.ico`. Candidate for a small separate Task.
- **Idea for later (User, animation session):** headers load from left to right, starting in IBM Plex Mono and changing to Literata. Not part of this session. Plex Sans stays the body font.
- **Branches:** `main` is the only branch. It is ahead of `origin/main` by 16 local commits, not pushed. Pushing is the User's decision.
- **Session status:** all three Stages are complete. No Worker has active work. The Worker's bus files are empty.
- **Stack stance (User):** lightweight by default, vanilla first. Any framework needs User approval, justified against technical debt. Recorded in the Spec's Notes.

## Tasks

**Stage 1:** Complete — [Summary](memory/stage-01/stage-01.summary.md)

**Stage 2:** Complete — [Summary](memory/stage-02/stage-02.summary.md)

**Stage 3:** Complete — [Summary](memory/stage-03/stage-03.summary.md)

## Workers

| Agent | Conversation | Not Read Since Handoff | Context |
|-------|--------------|------------------------|---------|
| site-agent | 1 | | |
