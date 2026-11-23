# APM Tracker

## Notes

- Session 2 starts from `main` at `ca642de`, with a clean tree and no other branches. The local server on port 8000 (pid 98390) serves the repository root.
- The User asked for the `Co-Authored-By` trailer to be removed. The 17 unpushed session 1 commits on `main` were rewritten to drop it, and their hashes changed. The three base commits, including `origin/main`, are unchanged. The old tip, `d536d1f`, is kept on `backup/before-trailer-removal`. Session 1 Spec and archive hash references were updated to the new hashes.
- **Pending: the switch's hover effect.** The User asked for the switch's shape to show on hover in animated mode. The exact effect is not decided. Confirm it with the User before the Task 2.3 prompt is written.
- **Pending: the session pause.** Stage 1 is complete. The session pauses before Stage 2, as the User decided. The content restructure and the wider redesign go to a later session. The User chooses when to summarize and archive, with `apm-8-summarize-session`.
- Branches `feat/literata-font` and `feat/animations-switch` are merged into `main` and kept. Deleting them is the User's call. Nothing is pushed.

## Tasks

**Stage 1:** Complete — [Summary](memory/stage-01/stage-01.summary.md)

**Stage 2:**

| Task | Agent | Status | Branch | Worktree | Next Action |
|------|-------|--------|--------|----------|-------------|
| 2.1 | site-agent | Ready | | | Held: session paused before Stage 2 for the User's decision |
| 2.2 | site-agent | Waiting: 2.1 | | | Await dependency |
| 2.3 | site-agent | Waiting: 2.2 | | | Await dependency; the User decides the hover effect first |

## Workers

| Agent | Conversation | Not Read Since Handoff | Context |
|-------|--------------|------------------------|---------|
| site-agent | 1 | | |
