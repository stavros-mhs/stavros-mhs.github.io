# APM Spec

## Overview

This APM session makes the personal site at https://stavros-mhs.github.io/ feel personal rather than generic, while keeping it minimal. It covers three things: a color system that gives the existing orange and grey palette more personality, a font chosen by the User from trials on localhost, and dark and light themes that follow the system setting on first visit, respect a saved choice and never flash the wrong theme. Success means the User reviews each branch on localhost and approves the result, and that normal text meets WCAG AA contrast (4.5:1) in both themes.

## Workspace

Working repository: `/home/stavros-mhs/Projects/stavros-mhs.github.io` (the only repository in the workspace).

```text
stavros-mhs.github.io/
├── index.html        # single page, edited
├── style.css         # all styles, edited
├── cv.pdf            # unchanged
├── README.md         # unchanged
├── AGENTS.md         # Worker rules, local only, never pushed
├── .apm/             # APM planning files, local only, never pushed
└── .claude/          # APM skills and guides, local only, never pushed
```

Read-only references:
- `https://agentic-project-management.dev/`: official APM site (confirmed by the User). Reference for the animation controls in session 2 only.
- `https://bigsky.software/cv/` and `https://joshblais.com/`: ideas only, not to copy.

Git:
- Base branch: `main`. Hosting: GitHub Pages from `main`'s root.
- One branch per Task, from `main`, named `type/short-description` (for example `fix/font-loading`).
- Commits follow `type: description`, using feat, fix, refactor, docs, test or chore.
- The Worker commits. The Manager merges into `main` only after the User approves the branch.
- Nothing is pushed to any remote, and nothing is merged into `main`, until the User says so.
- `AGENTS.md`, `.apm/` and `.claude/` are local-only and are never committed or pushed. They are listed in `.gitignore`, which is the repo's normal ignore file and may be committed. The listing names the folders but not their contents. The Planner set this up after the Rules were approved.

Local-only files: `AGENTS.md`, `.apm/` and `.claude/` stay on disk and are not tracked by Git. A commit is pushed as a whole, so a local Git history of these files would send them upstream with the next push. Excluding them is the only reliable way to keep them out.

Authoritative documents:
- `.apm/brief.md`: goal, must-haves 1 and 2 (for this session), stack, branch and push rules, out-of-scope list. Must-have items 3, 4 and 5 are deferred to later sessions.
- The User's answers in this planning conversation, recorded in the Notes and design sections below.

Existing `AGENTS.md`: none. It is created at the Rules step with an `APM_RULES` block, and it is local only.
Existing `CLAUDE.md`: none. No import is needed.

## Notes

**Session boundary.** This session covers colors, the font, and dark and light mode, including the first-visit theme flash. The following are deferred:
- Session 2: animations and the minimal mode that disables them. This includes smooth scrolling and the 0.5s theme transitions, which count as motion.
- Session 3: "Doing now" and "Listening now" sections.
- Later session: a personal blog, possibly as a new page. The User wants to host one at some point.

**Personality.** The User says personality comes from three things: the orange and grey palette, the font, and a personal voice in the writing. This session covers the first two. The writing voice is content and is deferred.

**Current-state findings** (verified by reading `index.html` and `style.css`):
- The Google Fonts `<link>` lacks `rel="stylesheet"`, so IBM Plex Sans never loads.
- The CSS requests `'Inter'`, so the page falls back to Roboto or Arial.
- The dark palette is defined twice (`:root` and `body.dark-mode`).
- `--base: #2e2e2e;;` has a doubled semicolon.
- The CSS header says "Catppuccin Frappé", but the palette is custom.
- The theme script runs at the end of `<body>`, so light-preference visitors see a dark flash.
- The light-mode link color `#b8874a` on `#f0f0f0` measures about 2.8:1 by hand calculation. This fails AA and must be fixed. The dark link `#c9a87c` on `#2e2e2e` measures about 6:1 and passes, but on the navbar's `#4a4a4a` it measures 3.96:1 and fails. Task 1.2 corrected both (see the palette table). The contrast script checks the values in place.

**Font trials.** Candidates were shown on localhost, not chosen in advance. The first round had IBM Plex Sans, Inter and Source Sans 3, which the User rejected. The second round had JetBrains Mono and Newsreader as references, plus 10 recommendations, all loaded from Google Fonts. The User first chose IBM Plex Mono, then chose IBM Plex Sans after comparing it with Mono and Literata. The Typography section records the final choice.

**Review loop.** When a branch is ready, the Worker reports it. The Manager or Planner then tells the User to refresh localhost, names what to look at, and asks for specific feedback.

**Stack stance (User, after planning).** The site stays lightweight and minimal. Plain HTML, CSS and vanilla JavaScript, with no build step, is the default. The User is not against a major framework, but does not want to inherit technical debt. Any framework must be justified against that concern and approved before it is added. Session 2 animations start with CSS, which needs no JavaScript, and add JavaScript only where the minimal mode toggle requires it.

**Scope latitude.** The User allows Workers to add or change files in the repository where a Task needs them, and to choose the best approach. Task Prompts name the files involved.

**Assumptions.** GitHub Pages serves `main`'s root. The User has not specified otherwise.

## Palette and Color

The palette keeps the orange and grey. The change is in how the color is used, so the page gets more personality without losing its restraint.

Values as implemented by the end of Task 2.2, after the User's review. Dark mode is unchanged since Task 1.2.

| Token | Dark | Light | Role |
|-------|------|-------|------|
| `--bg` | `#2e2e2e` | `#f4ecdc` | page background (light: cream paper) |
| `--surface` | `#4a4a4a` | `#e8dcc6` | navbar, toggle track |
| `--text` | `#e0ddd8` | `#5a4c3c` | body text |
| `--text-muted` | `#a8a49e` | `#6a5b49` | secondary text, contact icons |
| `--accent` | `#e8954d` | `#b5602e` | h1 and section headings (h2). Large text only, so 3:1 applies. |
| `--link` | `#d8b78e` | `#805832` | links. Must pass 4.5:1 on `--bg` and on `--surface`. |
| `--link-hover` | `#f5b06e` | `#8a3c12` | link hover and active. Must pass 4.5:1 on `--bg` and on `--surface`. |
| `--focus` | `#e8954d` | `#805832` | keyboard focus ring on the toggle. Must pass 3:1 on `--surface`. |
| `--rule` | `#3a3a3a` | `#d7c9ad` | horizontal rules |

Toggle tokens, defined once for both themes:

| Token | Value | Role |
|-------|-------|------|
| `--knob-dark` | `#2e2e2e` | knob in light mode, showing the moon: the mode you will switch to |
| `--knob-light` | `#f4ecdc` | knob in dark mode, showing the sun: the mode you will switch to |
| `--moon-icon` | `#e8954d` | moon icon, on the dark knob |
| `--sun-icon` | `#b5602e` | sun icon, on the cream knob |

The Spec's first values for `--link` and `--link-hover` in the dark theme failed on the navbar, so Task 1.2 changed them. Visited links use `--link` and active links use `--link-hover`.

Rules for use:
- `--accent` marks the h1 and the section headings (h2), in both themes. Link hover uses `--link-hover`, not the accent.
- Contact icons use `--text-muted`. The toggle icons and knob use the toggle tokens above.
- Each token is defined once per theme. The duplicate dark block is removed. The toggle tokens are defined once for both themes.

## Typography

- The page uses one typeface: IBM Plex Sans. The User chose it after comparing it with IBM Plex Mono (smaller heading) and Literata, the APM site's font. The User found Plex Sans cleaner and more readable. The candidates are kept in `.apm/memory/font-candidates.md`, so the font can be changed later.
- The loaded family and the CSS `font-family` must match. The Google Fonts `<link>` gets `rel="stylesheet"`.
- Font sizes, line height and the 600px breakpoint stay as they are unless a Task changes them for legibility.
- Headings and bold text use weight 500, not the browser's bold. Only the regular weight of the font is loaded, so the browser's bold was an imitation. The User accepted the lighter weight in both themes.

## Theme Behavior

- The theme is set on `<html>` as `data-theme="dark"` or `data-theme="light"`. An inline script in `<head>` sets it before the first paint, so there is no flash.
- Resolution order:
  1. A saved choice in `localStorage` under the key `theme`.
  2. The system setting, from `prefers-color-scheme`.
  3. Dark, if the system has no preference.
- The existing checkbox toggle stays. It saves the choice under the same key.
- The toggle's knob and icon show the mode you would switch to: a moon on a dark knob in light mode, and a sun on a cream knob in dark mode. The toggle can be reached and focused with the keyboard, and its focus ring uses `--focus`.
- Theme transitions are not changed in this session.

## Quality Checks

- Local serving: `python3 -m http.server` from the repository root.
- Both themes are checked with and without a saved choice, and with the system setting emulated in each mode.
- Mobile is checked at widths of 600px and below.
- Contrast: WCAG AA for normal text (4.5:1) and large text (3:1), in both themes. Measured by `tools/contrast_check.py`. The `tools/` folder is ignored by Git, so it is never committed or pushed. See `tools/README.md`.
- The User's localhost review is the final acceptance check for each branch.
