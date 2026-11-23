# Stage 1 - Foundations

## Outcome
- **Task 1.1, load the font correctly:** Partial, accepted by the Manager. Merged into `main` as `0e5b65a`. Worker: `site-agent`, conversation 1. The font loads and renders as IBM Plex Sans. The remaining unmet criterion is a console 404 for `/favicon.ico`, which predates this Stage. The User skipped it for now. A favicon is recorded as an idea for later.
- **Task 1.2, define palette tokens and fix contrast:** Partial, accepted by the Manager. Merged as `4a4e32f`. The tokens are defined once per theme, and all 14 contrast pairs pass. The one unmet criterion is "dark theme looks the same as before". The User accepted the deliberate changes. See the Findings below.
- **Task 1.3, set the theme before first paint:** Partial, accepted by the Manager. Merged as `3d2d52a`. The theme is set in `<head>` before the body appears. The one unmet criterion is browser Case 3 (no saved choice, system no preference), which headless Chromium cannot emulate. The User accepted it on the Node simulation.
- **Remaining from this Stage:** the favicon, the visual review of the palette (judged again in Stage 2), and Case 3 as a browser test.

## Findings and Decisions
- The Google Fonts link lacked `rel="stylesheet"`, so IBM Plex Sans never loaded. Fixed in Task 1.1. The `body` font-family now names IBM Plex Sans, with system fallbacks. Source: [1.1 log](task-01-01.log.md).
- The palette was defined twice, and the light link colour failed contrast at about 2.8:1. Tokens are now defined once per theme, under the names in the Spec. Source: [1.2 log](task-01-02.log.md).
- Some dark values changed, and the Spec's first dark link values failed on the navbar. The Spec's palette table now holds the implemented values. Source: [1.2 log](task-01-02.log.md).
- Visited links use the link colour, and active links use the hover colour. This is a deliberate change accepted by the User. Source: [1.2 log](task-01-02.log.md).
- The accent colour passes only as large text, 3:1. It is used on the h1 and h2 headings only. Source: [1.2 log](task-01-02.log.md).
- `--text-muted` is defined but not used until Task 2.2. Source: [1.2 log](task-01-02.log.md).
- The theme is resolved in `<head>` from the saved choice, then the system setting, then dark. The toggle saves the choice under `theme`. Source: [1.3 log](task-01-03.log.md).
- Headless Chromium here cannot emulate "no preference", so Case 3 was checked only by the Node simulation. The User accepted this. Source: [1.3 log](task-01-03.log.md).
- Browser test tooling sits outside the repository, in `site-tools`: a Playwright script for the theme checks and the contrast script. The Rules do not mention it yet, and adding it needs User approval.

## Validation and Limits
- **Stage verification (Manager, on the combined `main` at `3d2d52a`):**
  - Contrast script: PASS, 14 of 14 pairs, 7 per theme.
  - Grep: no old token names, no `dark-mode` or `body:not` hooks, no `;;`, no "Catppuccin" or "Inter".
  - Browser check (`theme_check.mjs`, headless Chromium): 20 of 20 checks pass. Case 3 is skipped, as recorded above. No console errors. Screenshots are in the scratchpad at `stage1-shots/`.
- **Limits:**
  - The aesthetic review of the palette is still with the User. It is judged again in Stage 2.
  - No pixel-level first-paint check was done. The "no dark frame" result rests on the ordering check, which confirms the theme is set before the body appears.
  - Case 3 is not tested in a browser.
  - The favicon 404 is open, and recorded as an idea for later.

## Task Logs
- Task 1.1: [Original](task-01-01.log.md)
- Task 1.2: [Original](task-01-02.log.md)
- Task 1.3: [Original](task-01-03.log.md)
