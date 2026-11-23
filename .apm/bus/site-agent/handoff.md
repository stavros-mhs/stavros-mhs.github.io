---
agent: site-agent
conversation: 3
handoff_log: ".apm/memory/handoffs/site-agent/handoff-02.log.md"
---

# site-agent Handoff Prompt - Conversation 3

Read first, in this order:
1. `.apm/memory/handoffs/site-agent/handoff-02.log.md`: the Handoff Log for conversation 2. It holds the design decisions, the test scripts and the known problems.
2. `.apm/memory/stage-02/task-02-05.log.md`: the Task 2.5 Task Log. It predates the follow-ups, so read the Handoff Log for those.
3. `.apm/memory/stage-02/task-02-04.log.md`: the Task 2.4 Task Log.
4. `AGENTS.md` (local-only): the Rules. Commit messages have no trailer lines.

## Current State
- **Tasks done:** Task 2.4 (phone sanity, `c5bb233`) and Task 2.5 (markdown transition and cd footer, `e1892ae`). Both are logged under `.apm/memory/stage-02/`.
- **Follow-ups since Task 2.5, with no Task Prompt:** the User's requests are committed as one work-in-progress commit, `f951538 feat: wip cd-style navigation and an orange prompt path`, on `feat/md-transition`. The Handoff Log lists them. The Task 2.5 Task Log and report do not cover them.
- **Branch and merge:** `feat/md-transition` is at `f951538`, on top of `e1892ae` and `c5bb233`. It is not merged. `main` is now `4aa93ae chore: show a work in progress page while the site is rebuilt`, which rewrites most of `index.html`. Merging will conflict. The User decides.
- **Bus files:** `task.md` is empty, so no Task Prompt is waiting. `report.md` was empty when this handoff was written. The Task 2.5 report was written in conversation 2, but its content is gone. The Manager's status is unknown. Rebuild it from `task-02-05.log.md` if the Manager needs it.
- **Unverified since `f951538`:**
  - `md-test.mjs`, `footnote-check.mjs` and `navbar-root-check.mjs`: the last run was interrupted, so the outcome is unknown.
  - `phone-check.mjs` at 360 and 390: not run since the cd change and the header colour.
  - `wrap-verify.mjs`: not run since the cd change. The new `cd` line on a file page may now wrap at 360px or 390px. Check it.
  - `contrast_check.py`: re-run to confirm the header colour.
- **Passing since `f951538`:** `pages-test.mjs` 32/32.
- **Local-only files changed:** `tools/contrast_check.py` (the prompt entry now tests `--prompt` at 4.5:1 on the surface). It is git-ignored, so do not commit it.
- **Untracked:** `contrast/` belongs to the Manager. Do not touch it.

## Next Action
No Task Prompt is pending. Resume with the follow-up work on `f951538`:
1. Re-run the unverified checks listed above, against the cd model. Fix only what they show is broken. Keep changes to `index.html` and `style.css`.
2. Report the results to the User: what passes, what fails, and what you changed.
3. Ask the User to decide the open questions in the Handoff Log:
   - keep or revert the cd model (decision 8), and folder-page expansion no longer changing the path (decision 7);
   - whether the dark-mode prompt needs a colour distinct from the directory names (decision 10);
   - the navbar name size change (decision 6);
   - the light-mode footer contrast (3.82:1, normal text);
   - the Animations switch overlap at 360px;
   - the minimal-mode link colour transition;
   - whether to rebase the branch onto the new `main`.
4. Commit only `index.html` and `style.css`, with no trailer lines. Flag any commit that is still work in progress. Do not merge, rebase or push.
5. Wait for the User's answers and instructions. Do not start new work without them.
