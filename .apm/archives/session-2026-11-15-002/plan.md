# APM Plan

This session sets the site in Literata, adds scroll-triggered motion with a letter reveal on headings, and adds an Animations switch that remembers the visitor's choice. It runs as six Tasks in three Stages, all on one Worker, because every Task edits `index.html` or `style.css`.

## Workers

| Agent | Domain | Description |
|-------|--------|-------------|
| site-agent | Front-end | Owns `index.html`, `style.css` and the motion check in `tools/`. Works in plain HTML, CSS and vanilla JavaScript. Commits on its Task branch. Does not push or merge. |

## Stages

| Stage | Name | Tasks | Agents |
|-------|------|-------|--------|
| 1 | Foundations | 1.1–1.2 | site-agent |
| 2 | Motion | 2.1–2.3 | site-agent |
| 3 | QA | 3.1 | site-agent |

## Notes

**Dispatch.** The six Tasks run in order on `site-agent`. Each Task edits the same two files, so nothing runs in parallel.

**Branch timing.** Each Task's branch is cut from `main` after the previous Task is merged, so no branch is stale. Session 1 used the same pattern.

**Review loop, per Task.**
1. The Worker commits on its branch and reports to the Manager.
2. The Manager tells the User which branch is ready, to refresh `http://localhost:8000`, and what to check.
3. The User sends specific feedback. The Worker applies it on the same branch.
4. The User approves. The Manager merges into `main` locally. Nothing is pushed.

**User actions.**
- Every Task: the User reviews on localhost in both themes and approves the merge.
- Tasks 1.2, 2.1, 2.2 and 2.3: the User judges the look and feel of the switch and the motion. The Worker reports what it checked and does not decide the look.
- Task 3.1: the User gives the final acceptance review in both modes.

**Local serving.** A `python3 -m http.server` already runs from the repository root on port 8000 (pid 98390). The Worker uses that server. It starts one only if port 8000 is free, and it does not start a second server on the same port.

**Shared files.** `index.html` and `style.css` change in every Task. Each Task starts from the latest `main`. The Manager merges each Task before the next one is cut.

**Integration points.**
- The head script in Task 1.2 sets `data-motion` before first paint. Tasks 2.1, 2.2 and 2.3 read that attribute.
- The `motion` key in `localStorage` is written by the switch in Task 1.2 and read by the head script in Task 1.2.
- The motion script that splits headings in Task 2.1 lives inline in `index.html`, so the page makes no extra request for it.

**Likely blockers.**
- The h1 has 52 characters. The step formula's 12ms floor makes its reveal about 1.2 seconds, a little over the Spec's one second. The Worker reports the measured duration. It does not change the floor without asking.
- The switch sits in a footer, so the navbar holds only its four links and the theme toggle. The navbar may still wrap at 390px. Task 1.2 checks the navbar and the footer at 390px.
- `transition: all` on the toggle circle and SVGs could override the minimal-mode rule unless the override uses `!important` and comes last in `style.css`. Task 2.3 checks it.
- Headless Chromium may not emulate "no preference" the way a browser does. If a check cannot run, the Worker reports it as skipped with the reason, as in session 1.

**Stage verification (Manager).**
- After Stage 1: the contrast script passes 16 of 16 pairs. The body and headings compute in Literata. The switch and the head script work in both modes.
- After Stage 2: the contrast script passes. A manual pass on localhost in both modes shows the reveal, the fades and the minimal behaviour. The 390px check has no horizontal scroll.
- After Stage 3: the motion check, the focus check, the theme check and the full QA matrix all pass, or any failure is reported with its observed behaviour.

**Work left for later sessions.** "Doing now" and "Listening now" are session 3. The personal blog and the favicon are later.

## Stage 1: Foundations

### Task 1.1: Set the site in Literata

- **Agent:** site-agent
- **Objective:** Make Literata the only typeface of the site, at weight 400 for text and 500 for headings and bold.
- **Dependencies:** None
- **Output:** `index.html` loads Literata 400 and 500 through a Google Fonts `<link rel="stylesheet">`, and the IBM Plex links are removed. `style.css` sets the body font stack to `'Literata', Georgia, 'Times New Roman', serif`. Headings and bold text use weight 500. The header comment in `style.css` stays accurate. Branch `feat/literata-font`.
- **Validation:** On localhost, the network panel shows the Literata request succeeding and no request for IBM Plex. The computed font of body text and of the h1 and h2 headings is Literata, and `document.fonts.check` reports it loaded. The contrast script passes 16 of 16 pairs in both themes. The Worker reports these results. The User reviews the page on localhost in both themes and approves the merge.
- **Guidance:** Spec, "Typography". Keep the palette, font sizes, line height and the 600px breakpoint unchanged. The text, links and content do not change. Literata's Latin file is one variable font served for both weights, about 39 KB.

1. In `index.html`, replace the Google Fonts link with `https://fonts.googleapis.com/css2?family=Literata:wght@400;500&display=swap`, with `rel="stylesheet"`.
2. In `style.css`, set the font stack on `body` to `'Literata', Georgia, 'Times New Roman', serif`, and check that headings inherit it.
3. Confirm that headings and `strong` use weight 500, and that no rule still names IBM Plex.
4. Serve the repository root and check the validation points above.
5. Commit as `feat: set the site in literata`.

### Task 1.2: Add the Animations switch and the saved mode

- **Agent:** site-agent
- **Objective:** Resolve the motion mode before first paint, and add a labelled switch that changes the mode and remembers it.
- **Dependencies:** Task 1.1 (Same-Worker)
- **Output:** An inline head script in `index.html` that sets `data-motion="animated"` or `data-motion="minimal"` on `<html>`, using the Spec's resolution order: the saved `motion` choice, then `prefers-reduced-motion: reduce` gives minimal, then animated. A switch in a footer, fixed at the bottom right of the screen, labelled "Animations". It is a `<button>` with `role="switch"` and `aria-checked`. It is hidden until the script shows it. Changing it sets `data-motion` at once and saves the choice under `motion`. Branch `feat/animations-switch`.
- **Validation:**
  - With no saved choice, the system in light, dark or no preference gives animated. With reduced motion requested, it gives minimal.
  - A saved `minimal` overrides a system without reduced motion, and a saved `animated` overrides a system with it. Both persist across reloads.
  - The switch moves Tab focus and shows the focus ring in both themes. Space and Enter change it.
  - With JavaScript disabled, the switch is not shown.
  - The theme toggle still works and still saves under `theme`.
  - At 390px the footer holds the switch, and the page has no horizontal scroll.
  - The contrast script passes 16 of 16 pairs.
  The User reviews the switch's look on localhost in both themes and approves the merge.
- **Guidance:** Spec, "Motion Modes and the Animations Switch". The switch's track and knob follow the theme toggle's appearance, with the knob on the right when animations are on. Keep the existing theme head script and the end-of-body toggle sync. Use the focus ring from the theme toggle (`--focus`, 2px, offset 3px). This Task adds the mode and the switch only. The effects that follow the mode come in Tasks 2.1 to 2.3, so the page looks the same in both modes after this Task, apart from the switch.

1. Add the head script after the theme script. Read `motion` inside a try block and resolve the mode as the Spec sets out.
2. Add the switch markup in a footer, fixed at the bottom right of the screen. Give it `hidden` in the markup, and have a small script remove `hidden` once the page has loaded.
3. Style the switch to match the theme toggle, with the focus ring from the Spec.
4. Wire the click to set `data-motion` and save `motion` in a try block.
5. Run each check in the Validation field and report the results.
6. Commit as `feat: add the animations switch and saved mode`.

## Stage 2: Motion

### Task 2.1: Reveal the h1 and section headings letter by letter

- **Agent:** site-agent
- **Objective:** Make each heading's letters appear left to right, blurred and spinning, then settle into place, in animated mode only.
- **Dependencies:** Task 1.2 (Same-Worker)
- **Output:** An inline script in `index.html` that splits the h1 and each section h2 into word groups and letter spans, and an `IntersectionObserver` that starts each heading when it becomes visible. CSS for the letters, word groups and the visually hidden full text. The h1 starts on page load, and the section headings start when they scroll into view. Branch `feat/letter-reveal`.
- **Validation:**
  - In animated mode, sampling the letters during the reveal shows them appearing one at a time from the left, with no gaps.
  - Each heading finishes in about one second. The Worker reports the measured time for the h1 and for one section heading.
  - Each heading ends in Literata, with no blur and no rotation.
  - The visually hidden text matches each heading exactly, and the split letters are `aria-hidden`.
  - Each heading starts once and does not replay when scrolled past again.
  - In minimal mode there are no split letters, and headings show as plain text.
  - With JavaScript disabled, the headings show as plain text.
  - At 390px no word breaks across lines, and there is no horizontal scroll.
  - The contrast script passes 16 of 16 pairs.
  The User reviews the reveal on localhost in both themes, in animated mode, and approves the merge.
- **Guidance:** Spec, "Letter Reveal". Use the step formula from the Spec. Give each heading `perspective` so the spin reads in 3D. Only `opacity`, `filter` and `transform` animate. Hide a heading with `visibility: hidden` only after the script has set `data-motion="animated"`, so a visitor without JavaScript sees the text. Keep the script inline. The h1 includes an em dash; treat it as a word.

1. Add the split function. For each heading, keep the text once in a visually hidden element, and build the visible letters inside an `aria-hidden` wrapper, one `.word` group per word, with spaces as text between groups.
2. Add the CSS for `.letter`, `.word`, the visually hidden element, and the start and end states from the Spec.
3. Add the `IntersectionObserver` with the threshold and root margin from the Spec. The h1 starts at once on load.
4. Confirm that minimal mode skips the split and that the headings stay plain.
5. Run each check in the Validation field and report the results, including the measured durations.
6. Commit as `feat: add the letter reveal to headings`.

### Task 2.2: Fade section text and draw the rules

- **Agent:** site-agent
- **Objective:** Fade each section's text in once, and draw each horizontal rule in from the left once, both on scroll and in animated mode only.
- **Dependencies:** Task 2.1 (Same-Worker)
- **Output:** CSS and a small addition to the observer from Task 2.1 that fades the text inside each `<section>` from opacity 0 to 1 over 0.6s, and scales each `hr` from 0 to 1 on the X axis over 0.8s. Both run once. Branch `feat/section-motion`.
- **Validation:**
  - In animated mode, section text goes from opacity 0 to 1 once, and each rule draws in once.
  - Scrolling past a section again does not replay either effect.
  - In minimal mode, all text and rules show at once.
  - Rules keep their `--rule` colour and 1px height, and the layout does not shift.
  - With JavaScript disabled, all text and rules show at once.
  - At 390px there is no horizontal scroll.
  - The contrast script passes 16 of 16 pairs.
  The User reviews the fades and rules on localhost in both themes, in animated mode, and approves the merge.
- **Guidance:** Spec, "Section Text and Rules". Reuse the observer from Task 2.1 with the same threshold and root margin. Text is hidden only after the script has set `data-motion="animated"`. Do not change the navbar or the contact icons. The `hr` rule currently uses `border-top`; change it to a 1px background element so that it can scale.

1. Add the fade and draw states to the CSS, scoped to `html[data-motion="animated"]`.
2. Add the rules to the observer so each section and each `hr` gets a class when it becomes visible, and stop observing it after that.
3. Confirm that the rules render at the same height and position in both modes.
4. Run each check in the Validation field and report the results.
5. Commit as `feat: fade section text and draw the rules`.

### Task 2.3: Set the theme fade and smooth scrolling by mode

- **Agent:** site-agent
- **Objective:** Keep the theme fade, the toggle motion, the link fades and smooth scrolling in animated mode, and turn them all off in minimal mode.
- **Dependencies:** Task 2.2 (Same-Worker)
- **Output:** CSS in `style.css` that keeps the existing 0.5s theme and toggle transitions, the Animations switch's background and track fade over 0.5s with the theme in animated mode, the 0.3s link fades and `scroll-behavior: smooth` in animated mode. In minimal mode, every element has `transition: none` and `html` has `scroll-behavior: auto`. Branch `feat/mode-motion`.
- **Validation:**
  - In animated mode, switching the theme shows a fade of about 0.5 seconds. The Worker samples the computed background during the fade.
  - In minimal mode, the same switch takes effect at once, with `transitionDuration` of `0s` on the body.
  - The Animations switch's background and track fade over about 0.5 seconds with the theme in animated mode, and change at once in minimal mode.
  - In animated mode, `html` computes `scroll-behavior: smooth`, and nav links scroll with a glide. In minimal mode it computes `auto`, and the page jumps.
  - The toggle knob and icons switch at once in minimal mode and animate in animated mode.
  - At 390px there is no horizontal scroll.
  - The contrast script passes 16 of 16 pairs.
  The User checks the theme fade and the scrolling in both modes on localhost, in both themes, and approves the merge.
- **Guidance:** Spec, "Mode Effects". Put the minimal rules at the end of `style.css` and use `!important`, so they override the `transition: all` rules on the toggle circle and SVGs. Do not change any colour value.

1. Add the minimal-mode rules at the end of `style.css`: `transition: none !important` for every element and its pseudo-elements, and `scroll-behavior: auto` on `html`.
2. Check that the animated-mode transitions in `style.css` are unchanged.
3. Run each check in the Validation field and report the results, with the sampled fade.
4. Commit as `feat: turn off motion in minimal mode`.

## Stage 3: QA

### Task 3.1: Run the motion, keyboard and mode checks

- **Agent:** site-agent
- **Objective:** Add the motion check the Spec requires, extend the keyboard check to the switch, and run the full QA matrix in both modes.
- **Dependencies:** Task 2.3 (Same-Worker)
- **Output:** `tools/motion_check.mjs`, a new headless Chromium check, and an update to `tools/focus_check.mjs` that also tabs to the Animations switch. `tools/README.md` describes both. A Task Report with the results of every check and every QA matrix cell. The tools folder is ignored by Git, so this Task makes no tracked change. It has no commit and no merge. Any fix to the site goes on its own `fix/...` branch, cut from `main`, as the Manager directs.
- **Validation:**
  - `tools/motion_check.mjs` passes these checks: animated is the default; reduced motion starts minimal; the switch changes the mode and saves `motion`; minimal has no split letters, no transitions and no smooth scrolling; animated letters appear left to right and end in Literata.
  - `tools/focus_check.mjs` passes for the switch, with a solid focus ring in both themes.
  - The contrast script passes 16 of 16 pairs.
  - The theme check and the Stage 2 keyboard check from session 1 still pass.
  - The QA matrix passes in every cell: both modes, both themes, and widths of 1280px and 390px, with no horizontal scroll at 390px and no console errors.
  - Any cell that cannot run is reported as skipped, with the reason.
  The User gives the final acceptance review on localhost in both themes and both modes.
- **Guidance:** Spec, "Verification". Follow the pattern of the existing tools: Playwright from `tools/node_modules`, and screenshots written to the scratchpad, not the repository. The repository root server on port 8000 is already running, so the Worker does not start another. Do not change the site in this Task unless a check fails and the fix is agreed with the Manager.

1. Write `tools/motion_check.mjs` for the checks in the Validation field.
2. Extend `tools/focus_check.mjs` so it tabs to the Animations switch and checks the focus ring.
3. Update `tools/README.md` with both tools and their limits.
4. Run the contrast script, the theme check, the focus check, the motion check and the QA matrix, and record each result.
5. Report the results. Commit nothing, because the tools folder is ignored by Git.
