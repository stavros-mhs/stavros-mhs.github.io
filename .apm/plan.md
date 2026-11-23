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
| 2 | Terminal landing (desktop) | 2.1–2.5 | site-agent |
| 3 | Desktop finish | 3.1–3.3 | site-agent |
| 4 | Minimal mode, mobile and QA | 4.1–4.3 | site-agent |

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

**Branch timing.** Each Task's branch is cut from `main` after the previous Task is merged. Task 2.1 starts from `main` at `4477350`.

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

## Stage 3: Hover and modes

### Task 3.1: The Animations switch shows its label and its state

- **Agent:** site-agent
- **Objective:** Place the word "Animations" above the switch, in a font size that makes the label as wide as the switch. The switch shows `ON` or `OFF` on its own track, in a colour that matches each mode in light and dark.
- **Dependencies:** Task 2.5 (Same-Worker)
- **Output:** The switch's markup and style in `index.html` and `style.css`. Branch `feat/switch-label`, cut from the merged `main`.
- **Validation:** Written when the Task is dispatched. The label and the switch are the same width. The `ON` and `OFF` text meets the 4.5:1 rule for normal text in both themes. The switch keeps its keyboard behaviour and its focus ring.
- **Guidance:** The switch stays fixed at the bottom right. Its track and knob keep their colour transitions, as the Spec says.

### Task 3.2: Hover states

- **Agent:** site-agent
- **Objective:** Add hover states for links, the terminal's directory names and the `source/` link, and the Animations switch's hover ring, for devices with hover, in animated mode.
- **Dependencies:** Task 3.1 (Same-Worker)
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
- **Dependencies:** Task 4.1 (Same-Worker)
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
