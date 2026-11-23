# APM Plan

This plan covers the motion session, re-planned on 19 Nov. The site becomes a terminal: the landing types `whoami` and `ls -l`, and the directories are navigable with `cd`, with each `.md` file opened as a subpage (see the Spec, "Terminal Landing"). Stage 1 is finished, and its two withdrawn Tasks are noted below. The motion rules that still fit are kept in Stages 3 and 4. Every Task runs on one Worker, `site-agent`, because each one edits `index.html` or `style.css`.

## Workers

| Agent | Domain | Description |
|-------|--------|-------------|
| site-agent | Front-end | Owns `index.html`, `style.css` and the motion, focus and QA checks in `tools/`. Works in plain HTML, CSS and vanilla JavaScript. Commits on its Task branch. Does not push or merge. |

## Stages

| Stage | Name | Tasks | Agents |
|-------|------|-------|--------|
| 1 | Headings and sections | 1.1, 1.2 and 1.4 done. 1.3 and 1.5 withdrawn | site-agent |
| 2 | Terminal landing (desktop) | 2.1–2.7 | site-agent |
| 3 | Desktop finish | 3.1–3.9 (order: 3.4, 3.7, 3.8, 3.9, 3.5, 3.6, 3.2, 3.3) | site-agent |
| 4 | Minimal mode, mobile and QA | 4.1–4.4 (4.4 runs before 4.2) | site-agent |

## Notes

**Re-plan, 19 Nov (User).** The name in `whoami` is `stavros-mhs`. Hobbies sit under `user/` and are not in the navbar. `ls -l` prints tree lines. Paths are short: `projects/aralloc/` and `projects/ginny/`. `cd ..` goes up one level. The whole site is the terminal. The re-plan runs from Stage 2, in this session.

**Defaults the Manager chose, for the User to confirm.**
- Routes use hash paths, such as `#/projects/aralloc/`, because they work on GitHub Pages as it is.
- The prompt is `~$` at the top and `~/projects/aralloc$` below it.
- The `ls -l` output ends with a count line, as the Unix `tree` command does.
- Markdown is shown as rendered text for the simple subset used (headings, paragraphs, lists, links, bold and code). If the User wants raw text, Task 2.3 changes.
- Without JavaScript, the landing shows its output as static text, and the navbar shows as links. Each `.md` file is a link to the raw file.
- The letter reveal (Task 1.1) and the section and rule reveals (Task 1.2) are retired, because the terminal replaces them. The User decides whether any of them survive on the path headings.
- `cd ..` is our own definition. joshblais.com has no `cd` command, so the User's reference is not that site.

**Withdrawn.** Task 1.3, the projects panel, which the terminal tree replaces. Task 1.5 moved to Task 2.4. The first draft of the landing (Task 2.3 in the earlier plan) was replaced by Tasks 2.1 to 2.3.

**Changes after the Task 2.1 review, 19 Nov (User).** Minimal mode is set aside and will be redefined later, probably as the page looked before the terminal. Task 3.2 waits for that. Below the terminal is removed. Its text moves first into the content files (`user/WHOAMI.md`, `user/HOBBIES.md`, `skills/SKILLS.md`, `contact/INFO.md` and the two `BRIEF.md` placeholders), which follow-up 01 creates. Task 2.3 therefore only renders and checks those files. The terminal now has a 1500 ms fade, a blinking cursor, and a start 400 ms after the fade. Commands type at human speed and output prints at machine speed.

**Task 2.2 accepted on 20 Nov, with the User's amendments.** Folders open in place, the command history is gone, file markdown prints below the terminal, and the navbar opens folder pages. The Spec's Terminal Landing describes the amended behaviour. Task 2.3 is covered by Task 2.2, since the six files are rendered and checked there. Stage 2 continues with Task 2.4, the phone sanity check.

**Priorities, 19 Nov (User).** Desktop first. A final desktop product comes before minimal mode and the mobile pass. For now, the page must not render badly at 390px (Task 2.4). Minimal mode is a scroll page with no terminal and no animations: each section is a header with the content of its `.md` file (Task 4.1). The detailed mobile pass is Task 4.2. Minimal-mode checks are removed from Stage 3.

**Dispatch.** Stage 2 runs in order: 2.1 (with follow-up 01), 2.2, 2.3, 2.4, then 2.5. Then Stage 3 (3.1 to 3.3), the desktop finish. Then Stage 4 (4.1 to 4.3): minimal mode, the mobile pass and QA. A colour Task follows after Task 2.4. Dark `--accent` becomes `#edab71`, which is 4.50:1 on the dark surface. Light stays `#b5602e`, as the User chose, and the Rules now allow 3:1 for headings at 24px and above.

**Branch timing.** Tasks 1.1 to 2.5 were cut from `main`. From 23 Nov, each Task's branch is cut from `dev` (at `f951538`) after the previous Task is merged into `dev`. When the site is finished, `dev` merges into `main`. Wherever a Task below says "the merged `main`", read `dev`.

**Cd model (User, 23 Nov).** The User kept the cd model of `f951538`, which has no Task. The Spec's Terminal Landing describes it, and it supersedes the Task 2.2 navigation wording. Task 2.5's output is the tree fade, the cat terminal move and the `cd ..` footnote, as `e1892ae` built them.

**Review loop, per Task.**
1. The Worker commits on its branch and reports to the Manager.
2. The Manager tells the User which branch is ready, to refresh `http://localhost:8000`, and what to check.
3. The User sends specific feedback. The Worker applies it on the same branch.
4. The User approves. The Manager merges into `main` locally. Nothing is pushed.

**User actions.**
- Every Task: the User reviews on localhost in both themes and approves the merge.
- Tasks 2.1 to 2.4 and 3.1 to 3.2: the User judges the look and feel. The Worker reports what it measured and does not decide the look.
- Task 4.1: the User gives the final acceptance review in both modes.

**Local serving.** Before starting, the Worker checks whether port 8000 already serves the repository root. If it does, the Worker uses that server. If the port is free, the Worker starts `python3 -m http.server` from the repository root. It never starts a second server on the same port.

**Shared files.** `index.html` and `style.css` change in almost every Task. Each Task starts from the latest `main`, and the Manager merges each Task before the next one is cut.

**Repository layout (from Task 2.3).** `user/`, `projects/`, `skills/` and `contact/` are real directories in the repository, holding the `.md` files. `projects/<name>/source/` is a link to GitHub, not a folder in the repository. The untracked `contrast/` page is the Manager's and is not part of the site.

**Integration points.**
- The head script on `main` sets `data-motion` before first paint. Every Task reads that attribute, and no Task changes the head script.
- The Animations switch stays fixed at the bottom right, as the Spec says.

**Likely blockers.**
- Hash routing must keep the browser's back button working, through `hashchange`. Task 2.2 reports what it sees.
- A `.md` file must be fetched from the same site, which GitHub Pages allows. The Worker reports any file that does not load.
- Headless Chromium may not report `(hover: hover)`. If a check cannot run, the Worker reports it as skipped with the reason.

**Stage verification (Manager).**
- After Stage 2: the landing and navigation pass in both modes at 390px and 1280px. `cd ..` returns to the previous state, and every `.md` file opens as a subpage. At 390px there is no horizontal scroll.
- After Stage 3: the contrast script passes. Hover and mode checks pass in both modes.
- After Stage 4: the motion check, the focus check, the theme check and the QA matrix pass, or any failure is reported with its observed behaviour.

**Work left for later sessions.** "Doing now" and "Listening now", the personal blog, the favicon, the writing voice, and loading speed, which the User wants touched up later.

## Stage 1: Headings and sections

- **Task 1.1** (letter reveal on headings) is done, committed as `feat: add the letter reveal to headings`, and merged at `3392d3b`. Its log is `.apm/memory/stage-01/task-01-01.log.md`. The letter reveal is retired by the re-plan, as noted above.
- **Task 1.2** (reveals of sections and rules) is done. It was reworked by the User's corrections, and its heading follow-up 01 is merged at `28810a8`. Its logs are `.apm/memory/stage-01/task-01-02.log.md` and `task-01-02.followup-01.log.md`. The section and rule reveals are retired by the re-plan.
- **Task 1.4** (navbar as `ls` output, with a phone panel) is done, and merged at `4477350`. Its log is `.apm/memory/stage-01/task-01-04.log.md`. Stage 2 replaces its navbar content and phone panel.
- **Task 1.3** (projects panel) is withdrawn. It never ran.
- **Task 1.5** (phone pass) moved to Task 2.4.

## Stage 2: Terminal landing

### Task 2.1: The terminal and the landing commands

- **Agent:** site-agent
- **Objective:** Put a terminal in the centre of the landing page. It types `whoami`, which prints `stavros-mhs`. It types `ls -l`, which prints the four top-level directories as tree lines. Then the navbar drops down from the top of the screen with the same four directories.
- **Dependencies:** Task 1.4 (Same-Worker)
- **Output:** In `index.html`, a terminal above the existing content, with the typing, output and navbar drop as the Spec describes. In `style.css`, the terminal's surface, the drop and the phone burger's panel, scoped to the motion modes. The old sections stay below the terminal for now. Branch `feat/terminal-shell`, cut from `main`.
- **Validation:**
  - At 1280px in animated mode, the terminal is centred, fades in, types `whoami`, prints `stavros-mhs`, types `ls -l`, and prints the tree in order. The Worker reports each step's time from `performance.now()`.
  - The navbar drops after the output, with its four names in order, each with its route as `href`.
  - At 390px, the terminal fits with a 16px gutter, and the burger opens a list of the four tree lines, each a link. No sideways scroll.
  - In minimal mode, the final output and the navbar show at once, with no transitions.
  - Without JavaScript, the output and the navbar links show, and there is no burger.
  - The text and the links pass 4.5:1 on the surface in both themes. The focus ring passes 3:1. The contrast script passes.
  - The QA matrix runs, and any failure is reported with its cause.
  - The Worker stops after reporting. The User checks the landing on localhost in both themes and both widths, and approves the merge.
- **Guidance:** Spec, "Terminal Landing". Use only existing palette tokens. The typing is text, and any cursor blink is opacity only. Do not change the palette, the font or the Animations switch.

### Task 2.2: Navigation, `cd ..` and the path

- **Agent:** site-agent
- **Objective:** Make the directories navigable. Clicking a directory prints its tree one level down. `cd ..` goes up one level. Clicking a `.md` file opens it as a subpage. The address bar shows the path after `#`.
- **Dependencies:** Task 2.1 (Same-Worker)
- **Output:** Routing in `index.html`, driven by `hashchange`, that prints each listing from the Spec's tree. `cd ..` at each level. The `.md` subpage, which shows the file's content. The `source/` link, highlighted, opens the repository in a new tab with `rel="noopener"`. Branch `feat/terminal-navigation`, cut from the merged `main`.
- **Validation:**
  - Every path in the Spec's tree prints its listing, at 1280px and at 390px.
  - `cd ..` returns to the previous state. The browser's back button follows the hash.
  - Each `source/` link goes to its repository in a new tab.
  - Each `.md` file opens as a subpage, with `cd ..` back to its folder.
  - Without JavaScript, each `.md` file is a link to the raw file.
  - In minimal mode, the new output appears at once, with no transitions.
  - The contrast script passes, and the QA matrix runs.
  - The Worker stops after reporting. The User clicks through the tree on localhost, in both themes, and approves the merge.
- **Guidance:** Spec, "Terminal Landing". Show each click as a typed command in the history, such as `cd projects/`, then its output. Keep the tree text exactly as the Spec gives it.

### Task 2.3: The content files and pages

- **Agent:** site-agent
- **Objective:** Move the content into real files, and remove the old sections from the landing page.
- **Dependencies:** Task 2.2 (Same-Worker)
- **Output:** These files in the repository: `user/WHOAMI.md`, `user/HOBBIES.md`, `skills/SKILLS.md`, `contact/INFO.md`, `projects/aralloc/BRIEF.md` and `projects/ginny/BRIEF.md`. The first four hold the text of the sections they replace, unchanged. The two `BRIEF.md` files hold placeholders, marked `PLACEHOLDER`, until the User writes them. The old sections are removed from `index.html`. Branch `feat/terminal-content`, cut from the merged `main`.
- **Validation:**
  - Each file's text matches the section it replaces, character for character.
  - The old sections are gone from the landing, and every path still works.
  - The subpages render the markdown subset, in both themes at 1280px and 390px.
  - At 390px, no sideways scroll, and no text clipped.
  - The contrast script passes, and the QA matrix runs.
  - The Worker stops after reporting. The User checks the content on localhost, and approves the merge.
- **Guidance:** The Task Prompt names this move, as the Rules require for text changes. Do not change the wording of any text.

### Task 2.4: Phone sanity check (details later)

- **Agent:** site-agent
- **Objective:** Make sure the page does not render badly at phone widths: no sideways scroll, nothing overlapping, and all text readable. Do not refine the layout. The detailed mobile pass is Task 4.2, after the desktop product is final.
- **Dependencies:** Task 2.3 (Same-Worker)
- **Output:** Fixes in `index.html` and `style.css` only, on branch `feat/phone-fixes`, cut from the merged `main`. The Task Log lists each check and each fix, with the widths before and after.
- **Validation:**
  - At 360px, 390px, 600px and 601px, in both themes and both motion modes: no sideways scroll, no clipped or overlapping text, and no path or tree line cut off.
  - The burger list fits at 390px.
  - The intro stays at 24 characters per line at 390px. The User accepts this with JetBrains Mono.
  - The Animations switch and the 12px listing text stay as they are.
  - The contrast script passes, and the QA matrix passes.
  - The Worker stops after reporting. The User checks the phone layout on localhost, and approves the merge.
- **Guidance:** Keep the 600px breakpoint and the palette. Do not change the font or any text.

### Task 2.5: Markdown pages: the tree fades out, the cat terminal moves up, and a `cd ..` footer

- **Agent:** site-agent
- **Objective:** When a `.md` file is clicked, the tree terminal fades out and is replaced by a terminal that shows the `cat` on that path. That terminal moves up smoothly, so the markdown prints below it. At the end of the markdown, a footer note on the right reads `> cd ..`, in the directory colour. Clicking it goes back to the folder.
- **Dependencies:** Task 2.2 (Same-Worker)
- **Output:** The page transition and the footer in `index.html` and `style.css`. Branch `feat/md-transition`, cut from the merged `main`.
- **Defaults to confirm:** the tree fades out over 250 ms, the cat terminal moves up over 400 ms with ease-out, and the markdown blocks follow the output pace. In minimal mode the page shows at once.
- **Validation:** Written when the Task is dispatched.
- **Guidance:** Animate only `opacity` and `transform`. The move is a `transform`. The `> cd ..` footer uses `--dir` and `--dir-hover`.

### Task 2.6: Rebuild the local checks and verify the cd model (added 23 Nov)

- **Agent:** site-agent
- **Objective:** Re-create `tools/` (lost with the move to a new machine), then verify `feat/md-transition` at `f951538` and fix only what the checks show is broken. Report the open questions with measured facts, without deciding them.
- **Dependencies:** Task 2.5 (Same-Worker)
- **Output:** `tools/` at the workspace root (local only), any fixes committed on `feat/md-transition`, a Task Log and a Report. Worktree `.apm/worktrees/feat-md-transition`.
- **Validation:** As in the Task Prompt. Task 4.3 later extends these tools instead of re-creating them.
- **Guidance:** The cd model stays. Colours, text and the navbar size are not changed here.

### Task 2.7: Contrast of small links, and the switch on phones (added 23 Nov)

- **Agent:** site-agent
- **Objective:** Apply the User's decisions from the Task 2.6 review: light-mode footnote and sub-24px navbar names use `--dir-hover` at rest, and on phones the Animations switch moves into the page flow.
- **Dependencies:** Task 2.6 (Same-Worker)
- **Output:** Changes in `index.html` and `style.css` on `feat/md-transition`, in the existing worktree.
- **Validation:** All of `tools/` re-run: `contrast_check.py` 0 failed, `phone-check.mjs` 0 failed at 360 and 390, other checks unchanged. Desktop switch position unchanged.
- **Guidance:** Spec, "Motion Modes and the Animations Switch". Colours only from existing tokens. The dark prompt, the navbar size and the minimal-mode transition (Task 3.3) stay as they are.

## Stage 3: Hover and modes

### Task 3.1: The Animations switch shows its label and its state

- **Agent:** site-agent
- **Objective:** Place the word "Animations" above the switch, in a font size that makes the label as wide as the switch. The switch shows `ON` or `OFF` on its own track, in a colour that matches each mode in light and dark.
- **Dependencies:** Task 2.5 (Same-Worker)
- **Output:** The switch's markup and style in `index.html` and `style.css`. Branch `feat/switch-label`, cut from the merged `main`.
- **Validation:** As in the Task Prompt (23 Nov). The label and the switch are the same width. The `ON` and `OFF` text meets the 4.5:1 rule for normal text in both themes. The switch keeps its keyboard behaviour and its focus ring. Branch cut from `dev`.
- **Guidance:** The switch stays fixed at the bottom right. Its track and knob keep their colour transitions, as the Spec says.

### Task 3.2: Hover states

- **Agent:** site-agent
- **Objective:** Add hover states for links, the terminal's directory names and the `source/` link, and the Animations switch's hover ring, for devices with hover, in animated mode.
- **Dependencies:** Task 3.1, Task 3.4, Task 3.5 and Task 3.6 (Same-Worker)
- **Output:** Rules in `style.css` inside `@media (hover: hover)`. The link underline is an `::after` line that scales from the left, in `currentColor`, and it replaces the `text-decoration` underline on hover and active. This Task also moves the site-wide `a:hover` rule inside the media query. The switch gets a 1px `--link` ring, with a 0.3s opacity fade, in animated mode. Branch `feat/hover-states`, cut from the merged `main`.
- **Validation:**
  - In animated mode, hovering a text link shows an underline that grows from the left over about 0.3 seconds.
  - Hovering a directory name shows the same underline.
  - Hovering the switch fades in a 1px ring in `--link`. The focus ring still shows on keyboard focus.
  - The Worker reports the contrast ratios of `--link` on `--bg` and on `--surface`.
  - If headless Chromium does not report `(hover: hover)`, the Worker reports the hover checks as skipped, with the reason.
  - The contrast script passes. The User reviews the hover states on localhost, in animated mode, and approves the merge.
- **Guidance:** Spec, "Hover States". Only `transform` and colour change. The contact-row icon lift is dropped, because the contact list is now a text file, unless the User wants icons on `INFO.md`.

### Task 3.3: Theme and switch fades, smooth scrolling (desktop, animated mode)

- **Agent:** site-agent
- **Objective:** Keep the theme fade and the switch's fade, and set smooth scrolling for animated mode on desktop. Minimal mode is Task 4.1.
- **Dependencies:** Task 3.2 (Same-Worker)
- **Output:** In `style.css`: the switch's background, track and knob change colour over 0.5s in animated mode. Smooth scrolling is set only under `html[data-motion="animated"]`. A block at the end of `style.css` sets `transition: none !important` and `animation: none !important` on every element, `::before` and `::after` under `html[data-motion="minimal"]`. Branch `feat/mode-motion`, cut from the merged `main`.
- **Validation:**
  - In animated mode, the body background fades over about 0.5 seconds on a theme change. The Worker samples the computed background during the fade.
  - In minimal mode, the same change is instant, with `transitionDuration` of `0s`.
  - In animated mode, `html` computes `scroll-behavior: smooth`. In minimal mode it computes `auto`.
  - In minimal mode, every hover effect from Task 3.1 has a `transitionDuration` of `0s`.
  - No colour value has changed.
  - At 390px there is no horizontal scroll.
  - The contrast script passes. The User checks the theme fade and the scrolling in both modes, and approves the merge.
- **Guidance:** Spec, "Theme Transitions and Smooth Scrolling" and "Mode Effects". The minimal rules go at the end of `style.css`, with `!important`. Do not change any colour value.

### Task 3.4: The new prompt appears at once after a cd (added 23 Nov)

- **Agent:** site-agent
- **Objective:** After a `cd` line finishes, the prompt of the folder it moved to appears instantly, as machine output, and then the next command (`tree`) types at human speed. Today the new prompt and the `tree` line are hidden, then revealed with the 250 ms fade at the same moment the first character of `tree` types (`buildTreePage` in `index.html`, the `revealFn(cmd, true)` at `treeAt`, then `typeActions(cmd, 'tree', treeAt)`).
- **Dependencies:** Task 3.1 (Same-Worker)
- **Output:** The fix in `index.html` (and `style.css` only if the fade needs it), on a branch `fix/cd-prompt-instant` cut from `dev`.
- **Validation:** Written when the Task is dispatched. In animated mode, after a `cd` from every starting folder to every folder (navbar, `cd ..`, `cd ~`, Back and Forward), the new prompt is in the page in full, with no fade, in one frame, after the existing 300 ms pause, and the first character of `tree` follows one typing interval later. The same holds for any other command that starts on a new prompt after a `cd`. Minimal mode is unchanged, and no other check changes.
- **Guidance:** Spec, "Terminal Landing", and the instant-prompt rule from Task 2.7 follow-up 01: the prompt is machine output, and only commands type at human speed.


### Task 3.5: Tab completion in the typed `cd` (added 23 Nov)

- **Agent:** site-agent
- **Objective:** Make the typed `cd` look like a person using a terminal's Tab completion. The user types the start of the folder, a Tab is pressed, and the machine completes it. Example, from the root: `cd ~/pro`, a beat, then `cd ~/projects/` appears at once. Then the Enter and the typed `tree` follow, as before.
- **Dependencies:** Task 3.9 (Same-Worker)
- **Output:** The change in `index.html` (and `style.css` only if needed), on a branch `feat/tab-complete` cut from `dev`.
- **Typing pace.** Unchanged: 90 to 170 ms per character. The User considered 55 wpm on 23 Nov and kept the current feel.
- **Defaults (confirmed by the User, 23 Nov):**
  - Prefix: the first three letters of each path segment that has more than three letters (`pro` for `projects`), or the whole segment if it has three or fewer.
  - Tab beat: about 250 ms of nothing, then the completion of that segment appears at once, including the `/`. For a nested path such as `~/projects/aralloc/`, each segment completes in turn: `~/pro`, Tab, `jects/`, `ara`, Tab, `lloc/`.
  - No completion for `cd ..` and `cd ~`, which have nothing to complete.
  - It also applies to the typed `cat` line on file pages (`cat WHO`, Tab, `AMI.md`). Say no to keep `cat` as it is.
  - The `tree` line after the cd is typed in full, without completion.
  - Minimal mode shows the whole command at once.
  - Two leftovers from Task 3.4's review, applied here for consistency with the machine-and-human rule: (1) a command's first typed character comes one typing interval after its own prompt, not in the same frame, for the `cd` line, the error page's `cd nope/` line and `tree` after no `cd`; (2) the landing's second prompt, before `tree` and after `whoami`'s output, appears instantly instead of fading in. The end-of-tree prompt keeps its fade with the first tree line, as the User set in Task 2.1 follow-up 04.
- **Validation:** Written when the Task is dispatched. In animated mode the typed characters keep the 90 to 170 ms pace, the beat before the completion is measured, the completion is in the page in one frame, and the final command text is exactly what it was before. Each route into a folder or file is checked, from every starting folder.
- **Guidance:** The machine-speed rule: the completion is machine output and appears instantly, with no fade or typing. The cursor follows the end of the text. The typing and completion must not change the final text or the pause that follows.

### Task 3.6: The Animations switch as a full-face button (added 23 Nov)

- **Agent:** site-agent
- **Objective:** Replace the knob and track of Task 3.1 with a full-face button: no knob and no slide. `ON` or `OFF` is engraved across the whole face in the mode's accent colour. The label "Animations" stays above, as wide as the button.
- **Dependencies:** Task 3.5 (Same-Worker)
- **Output:** The markup in `index.html` and the style in `style.css`, on a branch `feat/switch-face` cut from `dev`.
- **Defaults (confirmed by the User, 23 Nov):**
  - Colour: `--prompt` (`#edae77` dark, `#934e25` light), 4.61:1 on `--surface`, in both states. `--accent` in light mode is 3.31:1 and fails for normal text.
  - Engraved: a static `text-shadow` of 1px, a highlight below and a shade above, in the same style as the track's inset shadow. No animation.
  - Size: the face keeps the track's size (80 by 40px on desktop, 60 by 30px on phones), so the label matches it as in Task 3.1. The text is about 15% of the width, 12px on desktop and 9px on phones.
  - The word swaps with no animation. The colour fade of the face is added in Task 3.3, with the other switch fades.
  - The button's corner radius is the pill again (999px), as the face is a pill. The 16px radius of Task 3.1 applies to the label's container only if it still has one.
- **Validation:** Written when the Task is dispatched. The face text reaches 4.5:1 in both themes and both states, the label equals the face's width, and keyboard, focus ring, accessible name and saved mode are unchanged. Phones cover nothing, as in Task 3.1.
- **Guidance:** Spec, "Motion Modes and the Animations Switch". No knob element remains.

### Task 3.7: The landing terminal sits at the same height as on other pages (added 23 Nov)

- **Agent:** site-agent
- **Objective:** The terminal on the landing appears at the same height as on folder and file pages, so that clicking the navbar does not make it jump.
- **Dependencies:** Task 3.4 (Same-Worker)
- **Output:** The change in `style.css` (and `index.html` only if needed), on a branch `feat/terminal-top` cut from `dev`.
- **Defaults to confirm (User):** the landing moves to the folder pages' position (the top), not the other way. It applies on desktop and phones, in both motion modes. The desktop negative margin on `.landing` that centred the terminal (Task 2.1 follow-up 03) is removed or replaced. The terminal's top edge is fixed, and it grows only downward as lines appear: no vertical centring, and no layout that shifts the top as the height changes (User, 23 Nov). Nothing else changes: the order of the landing's output, the navbar drop and the fades.
- **Validation:** Written when the Task is dispatched. The terminal box's top, measured with `getBoundingClientRect()`, is the same, within 1px, on the landing and on every folder and file page, at 1280, 800, 601, 390 and 360px wide, in both themes and both modes. The top is also sampled from the first frame to the end of the landing's animation, and while a folder opens in place, and it never moves, within 1px, while the bottom edge grows. No sideways scroll, and no overlap with the navbar or the switch.
- **Guidance:** Spec, "Terminal". The Animations switch and the footnote positions do not change.

### Task 3.8: Clear the screen, then type and execute the cd (added 23 Nov, revised the same day)

- **Agent:** site-agent
- **Objective:** When the user clicks the `> cd ..` footnote or a navbar name, the screen first clears (fades out to an empty terminal at the same top). Then the current prompt appears, the `cd` is typed and executed, the destination's prompt appears, and the next command types. The move to the root, or to any previous state, is seamless.
- **Dependencies:** Task 3.7 (Same-Worker)
- **Output:** The change in `index.html` (and `style.css` only if needed), on a branch `feat/cd-in-place` cut from `dev`. It replaces the part of the cd model where the new page types the `cd` line.
- **Sequence (User, 23 Nov, chosen from three options):**
  1. Click. The terminal's content, the markdown and the footnote fade out together over 250 ms to an empty terminal, which stays at the same top (Task 3.7).
  2. The current prompt appears at once, for example `~/projects/ginny$`.
  3. `cd ..` is typed at human speed (90 to 170 ms per character, the first one an interval after the prompt), then a 300 ms pause.
  4. The new prompt appears at once, for example `~/projects$`.
  5. `tree` (or `cat`) types, and its rows fade in as they do now.
- **Defaults to confirm (User):**
  - It applies to the footnote's `cd ..` and to the navbar names, including the `~/` item. Clicking a folder in the tree still expands in place with no `cd`.
  - The `cd` command is the same as now (`cd ..`, `cd ~`, `cd ~/<folder>/`). When the destination is the folder the prompt is already in (a file page going back to its folder), no `cd` is typed: clear, prompt, `tree`.
  - A second click while the sequence runs finishes it at once and goes on. Back, Forward, typed addresses and a direct load have no terminal to clear and type in, so they change the page at once, as now. Minimal mode changes the page at once.
  - Keyboard use (Enter on the link) behaves like a click.
  - Cleanup that Task 3.7 left: the file page's terminal move is now a no-op that still waits 400 ms. This Task removes `moveTo`, the `term-move` styles, the 400 ms wait and the dead `is-top` class, since it reworks the same transition code. The markdown blocks, footer and fades keep their timing.
- **Validation:** Written when the Task is dispatched. In animated mode, for every route in the defaults and from every starting page, the order and the timing of the five steps are sampled: the 250 ms fade, the instant prompts at full opacity, the typing pace, the 300 ms pause. The terminal's top never moves. The new page shows no repeated `cd` line. The final text of every page is unchanged.
- **Guidance:** Spec, "Clear, then cd". Machine output is instant, human typing is typed, the fade-out is the clear. Do not change the pace.

### Task 3.9: A smooth clear, and a box that follows its content (added 23 Nov)

- **Agent:** site-agent
- **Objective:** Rework the clear sequence of Task 3.8 as the User wants: the text fades out inside the box, the box resizes smoothly to the empty box, then the `cd` is typed, then the destination's rows arrive with the box growing smoothly row by row. The box never holds a stale or fixed height, and no text is ever outside it.
- **Dependencies:** Task 3.8 (Same-Worker). Task 3.8 is not merged yet, so this Task's branch `feat/smooth-clear` is cut from `feat/cd-in-place` at `cc60a86`.
- **Output:** Changes in `index.html` and `style.css`, on `feat/smooth-clear`. The Rules (`AGENTS.md`, Motion) now allow the terminal box's `height` to transition in animated mode, over 250 to 300 ms with ease-out, as the User asked.
- **Validation:** Written when the Task is dispatched.
- **Guidance:** Spec, "Clear, then cd". Replace the fixed-height lock of Task 3.8 follow-up 01 (`holdHeight`), which let text spill out of the box. Machine output is instant, human typing is typed.

## Stage 4: Minimal mode, mobile and QA

### Task 4.1: Minimal mode

- **Agent:** site-agent
- **Objective:** Build minimal mode as a scroll page: no terminal and no animations. Each section is a header, which is the file's path, and the content of its `.md` file, in this order: `user/WHOAMI.md`, `user/HOBBIES.md`, `projects/aralloc/BRIEF.md`, `projects/ginny/BRIEF.md`, `skills/SKILLS.md`, `contact/INFO.md`.
- **Dependencies:** Task 3.3 (Same-Worker)
- **Output:** The minimal layout, shown when `data-motion="minimal"`. The Task Prompt decides with the User how the content reaches the page, either by fetching the files or by writing it in, and how the navbar behaves in this mode. Branch cut from the merged `main`.
- **Validation:** At 1280px in minimal mode, no terminal and no animations are present. The six sections appear in the order above, each with its path as its header. Their content matches the files. Report `transitionDuration` and `animation` on the sections as `0s` and `none`. The mobile layout is checked only in Task 4.2.
- **Guidance:** The Task Prompt is written when this Task is due. Do not start it before the desktop product is final.

### Task 4.2: Mobile pass

- **Agent:** site-agent
- **Objective:** Check every mode at phone widths, and refine the phone layout, the terminal, the burger, and minimal mode. When the burger's panel opens, blur the page behind it. The blur fades in with `opacity` only.
- **Dependencies:** Task 4.1 and Task 4.4 (Same-Worker). The User's choice from the Task 4.4 trial settles the phone terminal's text size or prompt
- **Output:** The fixes and refinements the User agrees, on a `feat/mobile` branch cut from the merged `main`.
- **Validation:** Written when this Task is due.
- **Guidance:** Written when this Task is due, with the User's decisions on the phone layout.

### Task 4.3: Motion, focus and QA checks

- **Agent:** site-agent
- **Objective:** Add the motion check the Spec requires, extend the focus check to the switch and the burger, run every check in both modes, and report the results.
- **Dependencies:** Task 3.3 (Same-Worker)
- **Output:** `tools/motion_check.mjs`, a new headless Chromium check. An update to `tools/focus_check.mjs` that also tabs to the switch and the burger. An update to `tools/README.md` that describes both. A Task Report with the result of every check and every QA matrix cell. The `tools/` folder is ignored by Git, so this Task makes no commit and no merge.
- **Validation:**
  - `tools/motion_check.mjs` passes: animated is the default; reduced motion starts minimal; the switch changes the mode and saves `motion`; minimal has no typing, no transitions and no smooth scrolling; the landing's output appears in order in animated mode; the navbar drops after the output; every hover effect appears in animated mode only.
  - `tools/focus_check.mjs` passes for the switch and the burger, with a solid focus ring in both themes.
  - The contrast script passes.
  - The QA matrix passes in every cell: both modes, both themes, widths of 1280px and 390px, no horizontal scroll at 390px, and no console errors.
  - Any cell that cannot run is reported as skipped, with the reason.
  - The Worker stops after reporting. The User gives the final acceptance review on localhost in both themes and both modes.
- **Guidance:** Spec, "Verification". Use Playwright from `tools/node_modules`, with screenshots written to the scratchpad, not the repository. Do not change the site in this Task unless a check fails, and then only with the Manager's agreement.

### Task 4.4: Trial of a smaller terminal font on phones (added 23 Nov, runs before Task 4.2)

- **Agent:** site-agent
- **Objective:** On phones, a command should never take more than one line (User, 23 Nov). Build a trial page with a smaller terminal font so the User can judge it on a phone, hosted at `/smaller-term-phone/` on the local server.
- **Dependencies:** Task 3.3 (Same-Worker)
- **Output:** An untracked preview in the repository root of the worktree, `smaller-term-phone/`, with its own copy of the site (`index.html`, `style.css` and the content files it fetches), so it opens at `http://localhost:8000/smaller-term-phone/`. Only phone widths (600px or below) change: the terminal's font size is smaller. The preview is never committed and never merged, and the Manager deletes it after the User decides. No change to the site's own files in this Task.
- **Validation:** Written when the Task is dispatched. It measures, at 360px and 390px, the largest font size at which every command line (`whoami`, `tree`, every `cd` line and `cat` line, from every starting folder, and the error page) fits on one line, and reports it with the longest line and its width. The preview serves two or three sizes the User can compare, for example through a query such as `?fs=15`, and the measured fit. Desktop is unchanged. Contrast is unchanged, because colours are.
- **Guidance:** Option 2 from the Tracker (a shorter prompt on phones) is not part of this trial unless the User asks. The result feeds Task 4.2. The font size is not animated.
