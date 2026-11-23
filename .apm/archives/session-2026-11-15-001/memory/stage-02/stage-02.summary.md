# Stage 2 - Personality

## Outcome
- **Task 2.1, font trials:** Success, accepted by the Manager. Merged into `main` as `715aa05`. The User first chose IBM Plex Mono. After comparing Plex Sans, Plex Mono with a 24px heading, and Literata on localhost, the User chose IBM Plex Sans. Worker: `site-agent`, conversation 1.
- **Task 2.2, accent roles:** Success, accepted by the Manager. Merged into `main` as `570208a`, nine commits, one file (`style.css`). The User reviewed both themes on localhost and approved the look. The result went beyond the Plan, at the User's request: section headings use the accent, the light palette is cream paper, the links are softer, headings and bold text use weight 500, and the toggle has a focus ring, a knob and icons that show the mode you would switch to.
- **Task 2.3, remove the trial page and confirm the font:** Success, accepted by the Manager. Merged into `main` as `578b282`. `trial-fonts.html` is deleted. The site uses IBM Plex Sans throughout, and no other font is requested.
- **Remaining from this Stage:** none. The favicon idea and the animation idea are for later.

## Findings and Decisions
- **Font:** IBM Plex Sans is the one typeface. The User chose it over Plex Mono and Literata after comparing them on localhost. Sources: [2.1 log](task-02-01.log.md), `.apm/memory/font-candidates.md` (local only).
- **Section headings use the accent, not the text colour.** This overrides the Plan's rule for headings. The Spec's palette rules are updated. Source: [2.2 log](task-02-02.log.md).
- **Light palette is cream paper,** with orange headings and brown links. The dark palette is unchanged. The Spec's palette table is updated. Source: [2.2 log](task-02-02.log.md).
- **The toggle must be focusable.** `display: none` removes an element from the tab order, so the checkbox is visually hidden instead. Its focus ring uses `--focus`. Source: [2.2 log](task-02-02.log.md).
- **The toggle shows the mode you would switch to:** a moon on a dark knob in light mode, and a sun on a cream knob in dark mode. The Spec's Theme Behavior section is updated. Source: [2.2 log](task-02-02.log.md).
- **Headings and bold text use weight 500.** Only the regular weight of the font is loaded, so the browser's bold was an imitation. The Spec's Typography section is updated. Source: [2.2 log](task-02-02.log.md).
- **The light links are close to the contrast limit.** `#805832` is 4.61:1 on the navbar, against the 4.5:1 minimum. Lighter links would need a lighter navbar. Source: [2.2 log](task-02-02.log.md).
- **Test tooling was updated.** The theme check expected the old light background, `#f0f0f0`. It now expects the cream `#f4ecdc`. This is a tooling change, not a site change.

## Validation and Limits
- **Stage verification (Manager, on the combined `main` at `578b282`):**
  - Contrast script: PASS, 16 of 16 pairs (8 per theme).
  - Theme browser checks (`theme_check.mjs`, headless Chromium): 20 of 20 checks pass. Case 3 (no saved choice, system no preference) is skipped, as accepted in Stage 1. No console errors apart from the known `/favicon.ico` 404 in earlier runs.
  - Keyboard checks (`focus_check.mjs`): 8 of 8 pass. Tab reaches the toggle, and its focus ring is solid in both themes.
  - Served files: `/` and `/style.css` return 200, and `/trial-fonts.html` returns 404.
  - Static: no `dark-mode` or `body:not` hooks, no `;;`, no removed token names, and no trial-page references.
  - Screenshots are in the scratchpad: `stage2-shots/` and `focus-shots/`.
- **Limits:**
  - Case 3 is untested in a browser, as accepted in Stage 1.
  - The palette's visual feel was judged by the User, not by a measurement.
  - The favicon 404 is open, and recorded as an idea for later.

## Task Logs
- Task 2.1: [Original](task-02-01.log.md)
- Task 2.2: [Original](task-02-02.log.md)
- Task 2.3: [Original](task-02-03.log.md)
