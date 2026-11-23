# APM Plan

This session gives the personal site a more personal color, font and theme system, in three Stages, one Task per branch, all handled by one Worker.

## Workers

| Agent | Domain | Description |
|-------|--------|-------------|
| site-agent | Front-end | Owns `index.html`, `style.css`, the temporary font trial page and the contrast script. Works in plain HTML, CSS and vanilla JavaScript. Does not push or merge. |

## Stages

| Stage | Name | Tasks | Agents |
|-------|------|-------|--------|
| 1 | Foundations | 1.1–1.3 | site-agent |
| 2 | Personality | 2.1–2.3 | site-agent |
| 3 | QA | 3.1 | site-agent |

## Notes

**Dispatch.** All seven Tasks run in sequence on one Worker, because every Task edits `index.html` or `style.css`. Nothing runs in parallel.

**Branch timing.** Each Task's branch is created from `main` only after the previous Task is merged. Each Task builds on the last, so a branch cut earlier would be stale.

**Review loop, per Task.**
1. The Worker commits on its branch and reports to the Manager.
2. The Manager tells the User which branch is ready, to refresh localhost, and what to check.
3. The User sends specific feedback. The Worker applies it on the same branch.
4. The User approves. The Manager merges into `main` locally. Nothing is pushed.

**User actions.** Two Tasks need the User's judgement:
- Task 2.1: the User picks the font from the trials.
- Every Task: the User reviews on localhost and approves the merge.

The Worker pauses at these points and does not decide them.

**Local serving.** Run `python3 -m http.server` from the repository root, then open `http://localhost:8000`. Font trials need internet access, because all candidates load from Google Fonts.

**Contrast script.** Task 1.2 creates it at `/home/stavros-mhs/Projects/site-tools/contrast_check.py`, outside the repository. It is never committed. It reads the token values from `style.css` and reports WCAG AA results for both themes. The Worker reports the output in its Task Report. The location is proposed for review, not yet confirmed.

**Known contrast failure.** The light-mode link color `#b8874a` on `#f0f0f0` measures about 2.8:1 by hand calculation. Task 1.2 fixes it. The dark link `#c9a87c` on `#2e2e2e` measures about 6:1 and passes.

**Local-only files.** `AGENTS.md`, `.apm/` and `.claude/` are listed in `.gitignore`. The Planner set this up after the Rules were approved. They are never committed or pushed. Workers do not commit, stage or push them, and do not edit `.gitignore`.

**Session 2 and 3.** Animations, the minimal mode, the "Doing now" section and the "Listening now" section are not in this Plan. They get their own sessions.

## Stage 1: Foundations

### Task 1.1: Load the font correctly

- **Agent:** site-agent
- **Objective:** Make the page load and render the font it declares, so the font choice in later Stages is real.
- **Dependencies:** None
- **Output:** `index.html` with the Google Fonts link set to `rel="stylesheet"`. `style.css` with `font-family` naming IBM Plex Sans, with the system fallbacks. The CSS header no longer says "Catppuccin Frappé".
- **Validation:** On localhost, the browser's network panel shows the font request succeeding. The computed font of body text is IBM Plex Sans. The browser console shows no errors from the page. The User is not needed.
- **Guidance:** Keep IBM Plex Sans for now; the trials come in Stage 2. The removal of the `;;` typo and the palette changes belong to Task 1.2. Spec, Design section "Typography".

1. Add `rel="stylesheet"` to the Google Fonts `<link>` in `index.html`.
2. Set `font-family` on `body` in `style.css` to `'IBM Plex Sans', Roboto, Arial, sans-serif`.
3. Replace the header comment in `style.css` with an accurate one.
4. Serve locally and check the network panel and computed font.
5. Commit as `fix: load the declared font and correct font-family`.

### Task 1.2: Define palette tokens once and fix contrast

- **Agent:** site-agent
- **Objective:** Replace the duplicated color blocks with one set of tokens per theme, matching the Spec's palette table, and make every text and link color pass WCAG AA.
- **Dependencies:** Task 1.1 (Same-Worker)
- **Output:** `style.css` with the Spec's tokens (`--bg`, `--surface`, `--text`, `--text-muted`, `--accent`, `--link`, `--link-hover`, `--rule`), each defined once per theme. The duplicate dark block is removed. The `;;` typo is fixed. The light link color is changed to a darker tan that passes 4.5:1. The contrast script exists at the path in the Notes.
- **Validation:** The contrast script reports a pass for normal text, links and link hover in both themes. The dark theme looks the same as before this Task, apart from the light link fix, which only affects light mode. The User is not needed.
- **Guidance:** Keep the current theme mechanism, `body.dark-mode` and `body:not(.dark-mode)`, for now. Task 1.3 changes it. Accent roles stay as they are until Task 2.2. Spec, Design section "Palette and Color".

1. Create the contrast script outside the repository, at the path in the Notes. It reads token values from `style.css` and computes WCAG contrast ratios.
2. Consolidate the dark values into one definition, using the Spec's dark column.
3. Consolidate the light values into one definition, using the Spec's light column.
4. Derive a light-mode link color that passes 4.5:1 on `--bg`. Record the chosen value and its ratio.
5. Remove the doubled semicolon.
6. Run the contrast script for both themes and fix any failing pair.
7. Commit as `refactor: define color tokens once per theme`.

### Task 1.3: Set the theme before first paint

- **Agent:** site-agent
- **Objective:** Pick the theme before the page paints, using the Spec's resolution order, so light-preference visitors never see a dark flash.
- **Dependencies:** Task 1.2 (Same-Worker)
- **Output:** An inline script in `<head>` of `index.html` that sets `data-theme` on `<html>`. CSS driven by `data-theme` instead of the body class. The checkbox toggle writes the choice to `localStorage` under the key `theme`. The end-of-body theme script is removed.
- **Validation:** With storage cleared and the system set to light, the page's first render is light, and no dark frame appears on reload. With storage cleared and the system set to dark, it is dark. With the system set to no preference, it is dark. A saved choice overrides the system setting and persists across reloads. The toggle still works. The User is not needed for these checks; the Worker reports each result.
- **Guidance:** The checkbox's checked state still needs syncing after the page loads. A small script at the end of `<body>` may do this, but it must not set the theme. Spec, Design section "Theme Behavior".

1. Add the inline `<head>` script that resolves the theme and sets `data-theme`.
2. Convert the theme CSS from `body.dark-mode` and `body:not(.dark-mode)` to `:root[data-theme="dark"]` and `:root[data-theme="light"]`.
3. Keep the checkbox in place. Sync its checked state to the resolved theme after the page loads.
4. Remove the old end-of-body theme script, keeping any code the toggle still needs.
5. Run each check in the Validation field and report the results.
6. Commit as `fix: set theme before first paint`.

## Stage 2: Personality

### Task 2.1: Font trials on localhost

- **Agent:** site-agent
- **Objective:** Show the same sample text in each candidate font on localhost, so the User can pick one.
- **Dependencies:** Task 1.3 (Same-Worker)
- **Output:** A temporary `trial-fonts.html` at the repository root, on this branch only. It shows the page's heading, a paragraph and a link in each of five candidates: IBM Plex Sans, Inter, Source Sans 3, Newsreader and JetBrains Mono. Each sample is labelled with its name and uses the page's theme tokens.
- **Validation:** All five fonts load on localhost, with network requests succeeding. The page shows each sample in both themes. **User action:** the User reviews the trial page on localhost and picks one font, or asks for another candidate. The Worker pauses until the User picks.
- **Guidance:** The trial page is a review aid and is removed in Task 2.3. Do not change `index.html` or `style.css` for the font choice in this Task. Spec, Design section "Typography" and Notes, "Font trials".

1. Create `trial-fonts.html` with the five labelled samples.
2. Load each candidate from Google Fonts in the page head.
3. Serve locally and check each font's network request.
4. Pause for the User's pick.
5. Commit as `feat: add font trial page`.

### Task 2.2: Apply the accent roles

- **Agent:** site-agent
- **Objective:** Use `--accent` only where the Spec says, so the page's color has deliberate emphasis instead of decoration.
- **Dependencies:** Task 2.1 (Same-Worker). Both Tasks change `style.css`, and the User reviews one change at a time.
- **Output:** `style.css` where `--accent` is used only on the h1, link hover and toggle focus. Other headings use `--text`. Contact icons use `--text-muted` unless they are interactive.
- **Validation:** The contrast script still passes in both themes. Pressing Tab until the toggle is selected shows the accent ring. **User action:** the User reviews on localhost in both themes and approves the look or asks for changes.
- **Guidance:** Spec, Design section "Palette and Color", "Rules for use". The starting values are a proposal; the User decides. The toggle's checkbox is hidden, so the focus ring belongs on its visible label, selected with `#toggle:focus-visible + .display label`. The toggle has no focus style today, so this is a new addition.

1. Replace `--accent` on h2 and contact icons with `--text` or `--text-muted`, per the Spec.
2. Keep `--accent` on the h1 and link hover. Add a visible focus ring in `--accent` on the theme toggle, so keyboard users can see when it is selected.
3. Run the contrast script for both themes.
4. Pause for the User's review and apply the feedback on this branch.
5. Commit as `feat: apply accent roles`.

### Task 2.3: Apply the chosen font and remove the trial page

- **Agent:** site-agent
- **Objective:** Make the font chosen in Task 2.1 the page's font, and remove the trial scaffolding.
- **Dependencies:** Task 2.1 and Task 2.2 (Same-Worker)
- **Output:** The Google Fonts link and `font-family` in `style.css` name only the chosen font. `trial-fonts.html` is deleted. No file in the repository references the trial page or the unused candidates.
- **Validation:** Body and heading text render in the chosen font, checked on localhost. `trial-fonts.html` no longer exists. Contrast still passes. **User action:** the User approves the page on localhost.
- **Guidance:** The chosen font name comes from the User's pick in Task 2.1. Spec, Design section "Typography".

1. Set the Google Fonts `<link>` to the chosen family only.
2. Set `font-family` on `body` and headings to the chosen family, with fallbacks.
3. Delete `trial-fonts.html`.
4. Check the rendering and contrast on localhost.
5. Commit as `feat: apply chosen font`.

## Stage 3: QA

### Task 3.1: Run the QA matrix and the contrast check

- **Agent:** site-agent
- **Objective:** Confirm the whole session against the Spec's quality checks, and report the results.
- **Dependencies:** Task 2.3 (Same-Worker)
- **Output:** A Task Report that lists the contrast script's results for both themes, and the result of each cell in the QA matrix below. Any fixes made are on their own `fix/...` branch, cut from `main` as the Manager directs.
- **Validation:** The contrast script passes for normal text (4.5:1) in both themes. Every cell of the matrix passes. **User action:** the User gives the final acceptance review on localhost.

QA matrix. Each cell is checked at both widths.

| Stored choice | System setting | Theme expected |
|---------------|----------------|----------------|
| None | Light | Light |
| None | Dark | Dark |
| None | No preference | Dark |
| Light | Dark | Light |
| Dark | Light | Dark |

Widths: above 600px and at 600px or below. That gives 10 checks in total.

1. Run the contrast script for both themes and record the results.
2. Work through each matrix cell in a browser on localhost, and record the result.
3. Report any failure with the cell and the observed behavior.
4. Pause for the User's final review.
5. Commit any fixes as `fix: ...` on their own branches.
