---
agent: site-agent
conversation: 2
stage: 2
---

# site-agent Handoff Log - Conversation 2

## Summary
Conversation 2 completed Task 2.4 (phone sanity, commit `c5bb233` on `feat/phone-sanity`, now in the history of `feat/md-transition`) and Task 2.5 (markdown transition and the cd footer, commit `e1892ae`, report sent). After Task 2.5 the User directed a long series of follow-ups with no new Task Prompt. They are committed as one work-in-progress commit, `f951538 feat: wip cd-style navigation and an orange prompt path`, on `feat/md-transition`. Nothing is merged or pushed. The follow-ups are not in any Task Log except the Task 2.5 log, which predates them.

Conversation 2 was compacted once. The Task 2.5 work and the follow-ups were rebuilt from the git history, `.apm/memory/stage-02/task-02-05.log.md`, the scratchpad test scripts, and the User's messages in the summary. Conversation 1's handoff is `.apm/memory/handoffs/site-agent/handoff-01.log.md`.

## Working Context

### Environment
- Site: static HTML, CSS and vanilla JS, `index.html` and `style.css` only. Serve from the repository root on port 8000 with `python3 -m http.server`. A server already runs on 8000 (python3, pid varies); never start a second one.
- Test scripts live in `/tmp/claude-1000/-home-stavros-mhs-Projects-stavros-mhs-github-io/42e77031-e143-4be7-aac8-06c42bbf9010/scratchpad/`. The scratchpad can be cleared, so re-create anything missing.
- Playwright is required from `tools/node_modules` through `createRequire`. PIL is available; numpy is not.
- `tools/` and `AGENTS.md` are git-ignored and local-only. `.apm/` and `.claude/` are local-only too. Do not stage or commit any of them. `contrast/` is untracked and belongs to the Manager. Do not touch it.
- Commit rules: `type: description`, lowercase, under 72 characters, no trailer lines. The User's memory says no `Co-Authored-By`, even if an attribution note asks for one. Stage only `index.html` and `style.css`.
- Do not merge, rebase or push. The User decides those.

### Scripts and what they check
- `pages-test.mjs`: 32 checks, the page model and the navigation sequences. Passes 32/32 after the cd change.
- `md-test.mjs`: 23 checks, the markdown transition, back, typed address, direct load, minimal mode, phone 390, keyboard, no-JS. Last run before the cd change: 23/23. Not re-run since.
- `footnote-check.mjs`: 40 checks, the `> cd ..` footnote on folder, error and file pages, and the click, keyboard, minimal and timing cases. Last run before the cd change: 40/40. Not re-run since.
- `navbar-root-check.mjs`: 30 checks, the `~/` item at eight widths, the root click, the phone panel and keyboard. Last run before the cd change: 30/30. Not re-run since.
- `phone-check.mjs 360,390`: the phone matrix, sideways scroll, overflow and overlap. The one known problem is the file-page footnote overlapping the Animations switch at 360px on first load (`#/user/WHOAMI.md`, both themes). Last run before the cd change.
- `wrap-verify.mjs`: checks the wrapped phone lines for breaks at spaces, the 2ch indent, no overflow and no split prompt. 10 wrapped lines, 0 problems, before the cd change.
- `prompt-orange-check.mjs`, `prompt-match-all.mjs`, `cmd-match.mjs`: the path, command and counter sizes and colours at 360 to 1280px, both themes.
- `cd-model.mjs`: replays the navbar, file, footnote, Back and root paths for the cd model.
- `nav-test.mjs`: obsolete. It tests the first Task 2.2 model, where a `cd ..` click typed a command. It times out on the first click. Do not use it.
- Contrast: `python3 -B tools/contrast_check.py style.css`. Last result PASS, before the header colour was added. The header uses `--prompt`, so it should still pass. Re-run to confirm.

### Design decisions and their reasons
1. **Hash routing.** The site is static. The server cannot serve `/projects/aralloc/`, so the route lives in the hash. The hash lets each folder and file have an address, supports Back, Forward and typed addresses, and lets a direct load open a page. `#/` is the root route, the same page as no hash.
2. **Scheduler.** `runActions(actions, onDone)` takes `{at, fn(instant)}`. Animated mode uses timers and sets `term-live`. Minimal mode runs `fn(true)` at once. `flushAll()` finishes pending actions, and `userAction()` calls it on any click. `untilFade: true` runs an action on the stage's opacity `transitionend`, with the timer as a fallback. This fixed a race in which the swap cancelled the fade.
3. **Markdown transition (Task 2.5).** The tree fades out (250ms), the cat terminal fades in (250ms) and types, then moves up with `translateY` (400ms, FLIP-measured). Blocks print 120ms apart, each fading in 250ms. The footer comes 250ms after the last block.
4. **Footnote `> cd ..`.** Every `cd ..` is now a right-aligned footnote below the terminal or the markdown, at the column's right edge, which is 760px wide. It is a plain link, not a code block. Reason: consistency. A code block would bring back the terminal's box look and read as copyable input.
5. **Navbar `~/` item.** It links to `#/` on desktop and in the phone panel. Reason: the root had no navbar entry.
6. **Navbar name size.** `min(1.5rem, 3.2vw)`. Below about 750px the names scale down. Reason: at 601px, five names had a 2px gap. The smallest gap is now 17px. This is an unreviewed decision the User was asked to review.
7. **Landing expansion.** Opening or closing a folder on the landing does not change the title, the prompt or the address. Reason: the User reported that the path showed the folder name while the listing was still the root's.
8. **Cd model (WIP, `f951538`).** Navigating to another folder types a `cd` first, in the folder the terminal was in: `cd ..` to the parent, `cd ~` to the root, or `cd ~/<folder>/` otherwise. Then the page runs `tree` at the new prompt. Within the same folder there is no `cd`, and the page runs `tree` at the current prompt. A file in another folder shows a `cd` line, then the `cat` line. Reason: the User said choosing `projects/` from the navbar should leave the terminal in `projects/`, not run `tree projects/` from root. The helper is `cdCommand(from, to)` in `index.html`, and `buildTreePage` and `buildFilePage` capture `from = pageFolder`. Reason for the expansion change: it applies decision 7 to folder pages too, so a folder page's path can no longer disagree with its listing. This changes the Task 2.2 behaviour the User had accepted, where clicking a folder on a folder page set the title.
9. **Tree header colour.** `.term-head` uses `--prompt`, so the folder's name is in the path colour on the root and on folder pages.
10. **Prompt.** The whole `.term-prompt`, including the `$`, uses the terminal's text size (`font-size: inherit`) and `--prompt`. Reason: the User wanted the path the same size as the command and the counter, and wanted it orange. The colour tokens are `--prompt: #edae77` (dark) and `--prompt: #934e25` (light), each at 4.61:1 on `--surface`. The old accent failed normal-text contrast at this size (3.73 dark, 3.31 light). In dark mode the prompt is now close to the directory colour `#edab71`. The User has not yet decided whether that matters.
11. **Phone wrap.** Under 600px, a terminal line wraps at spaces (`overflow-wrap: break-word`), with `padding-left: 2ch; text-indent: -2ch`, so continuation lines are indented. The prompt is `white-space: nowrap`. Reason: the User disliked mid-word wrapping. Measured widths: the cat line needs 422px and the error line 348px, against 296px at 360 and 326px at 390.
12. **Desktop scope.** The prompt change covers desktop as well as phones. The User asked for the path and counter to match on desktop, and the same rule applies everywhere.

### Known and accepted problems (not fixed)
- Light-mode footer `> cd ..` is `--dir` on the page background, 3.82:1. It passes the 3.31:1 target for directory names, but the footer is normal text and needs 4.5:1. Raised in Task 2.5, Important Finding 1. The User has not decided.
- Light-mode footer and dark-mode prompt colours (see decision 10) are open for the User's decision.
- Desktop navbar names are 24px, and the desktop tree is 21.28px. Pre-existing. Not changed.
- Minimal mode keeps a 0.3s colour transition on every link, from the base `a` rule. That rule is in `c5bb233`, so it predates this work. Task 2.5 asked for transitions of 0s in minimal mode.
- QA matrix (`tools/qa_matrix.mjs`) is not a gate until Task 4.3. It fails the two phone `h1` checks, and the line-length check errors. These predate this work. No section was restored.
- Animations switch overlaps the file-page footnote at 360px on first load. Reported, unresolved.

## Git state at handoff
- `feat/md-transition`: `f951538` (WIP, follow-ups), `e1892ae` (Task 2.5), `c5bb233` (Task 2.4 base). Not merged.
- `main` is now `4aa93ae chore: show a work in progress page while the site is rebuilt`, which rewrites most of `index.html`. Merging `feat/md-transition` into it will conflict. The User decides the merge.
- Working tree: clean except the untracked `contrast/`, which must not be touched.
