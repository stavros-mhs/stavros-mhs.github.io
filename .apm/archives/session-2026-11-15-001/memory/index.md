# APM Memory Index

## Notes

- **Headless Chromium cannot emulate "no preference".** The `prefers-color-scheme: no-preference` branch of the theme script is checked only by the Node simulation. Source: [Stage 1 summary](stage-01/stage-01.summary.md), [1.3 log](stage-01/task-01-03.log.md).
- **Navbar links sit on `--surface`.** Any link colour must pass 4.5:1 on `--surface` as well as on `--bg`, in both themes. The light links are at 4.61:1, close to the limit. Source: [1.2 log](stage-01/task-01-02.log.md), [2.2 log](stage-02/task-02-02.log.md).
- **The accent passes only as large text.** `--accent` passes 3:1 on both backgrounds, so it is limited to h1 and section headings (h2) and icons. Source: [1.2 log](stage-01/task-01-02.log.md), [2.2 log](stage-02/task-02-02.log.md).
- **A hidden element cannot take keyboard focus.** `display: none` removes the toggle from the tab order, so the checkbox is visually hidden instead. Source: [2.2 log](stage-02/task-02-02.log.md).
- **Only the regular weight of IBM Plex Sans is loaded.** Headings and bold text use weight 500, because the browser's bold was an imitation. Source: [2.2 log](stage-02/task-02-02.log.md).
- **`/favicon.ico` returns 404.** This is an idea for later, not a Task. Source: [1.1 log](stage-01/task-01-01.log.md).
- **Phone line length is 30 characters per line at 390px,** below the 45 to 90 target. Accepted as a limit. The desktop value of 55 is in range. Source: [3.1 log](stage-03/task-03-01.log.md).
- **Browser test tooling lives in `tools/`, ignored by Git.** It holds Playwright and the theme, keyboard, QA and contrast checks. It is documented in `tools/README.md`. The Rules do not mention it yet. The theme check expects the cream light background. Source: [Stage 1 summary](stage-01/stage-01.summary.md), [Stage 2 summary](stage-02/stage-02.summary.md).

## Stage Summaries

| Stage | Outcome | Summary |
|-------|---------|---------|
| 1 - Foundations | Complete. The font loads, colour tokens are defined once per theme, and the theme is set before first paint. Three Partial outcomes were accepted, and the remaining items are listed in the summary. | [Stage 1](stage-01/stage-01.summary.md) |
| 2 - Personality | Complete. The font is IBM Plex Sans. The accent roles, the cream light palette and the toggle are in place, and the trial page is removed. All three Tasks were accepted as Success. | [Stage 2](stage-02/stage-02.summary.md) |
| 3 - QA | Complete. The contrast, theme, phone and keyboard checks pass. Phone line length is 30 characters per line, accepted as a limit. | [Stage 3](stage-03/stage-03.summary.md) |
