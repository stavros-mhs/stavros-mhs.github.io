# APM Spec

## Overview

This APM session gives the personal site at https://stavros-mhs.github.io/ one typeface, Literata, and a set of motion effects that play as the visitor scrolls. The effects are a left-to-right letter reveal on the headings, a fade on section text and a rule that draws in between sections. A labelled Animations switch in the footer turns them off, and the visitor's choice is remembered. Success means every change is checked in both themes, both motion modes, at widths above and at 600px and below, and under both reduced-motion settings. Normal text must still meet 4.5:1 contrast, and the User approves each Task on localhost.

## Workspace

Working repository: `/home/stavros-mhs/Projects/stavros-mhs.github.io`, the only repository in the workspace.

```text
stavros-mhs.github.io/
├── index.html        # single page, edited
├── style.css         # all styles, edited
├── cv.pdf            # unchanged
├── README.md         # unchanged
├── AGENTS.md         # Worker rules, local only, never committed
├── .apm/             # APM planning files, local only, never committed
├── .claude/          # APM skills and guides, local only, never committed
└── tools/            # local test tools, ignored by Git, never committed
```

Read-only references: `https://agentic-project-management.dev/` is the APM project's site. Its home-page script respects `prefers-reduced-motion`, but no on/off control was found in its main script, so the switch is designed for this site. `https://bigsky.software/cv/` and `https://joshblais.com/` are ideas only, not to copy.

Git:
- Base branch `main`. Hosting is GitHub Pages from `main`'s root, after the User pushes. Nothing is pushed this session.
- One branch per Task, cut from `main`, named `type/short-description`, for example `feat/letter-reveal`.
- Commits use `type: description`, with type feat, fix, refactor, docs, test or chore. The Worker commits.
- The Manager merges into `main` only after the User approves the Task on localhost. Nothing is merged or pushed without the User's go.
- `.gitignore` already lists `.apm/`, `.claude/`, `AGENTS.md` and `tools/`. Those stay local, and no Worker edits `.gitignore`.

Authoritative documents:
- `.apm/archives/session-2026-11-15-001/brief-session-2.md`: the session 2 goal, the must-haves, the stack, the constraints, the verification and the working method. This Spec follows it, except where the Notes record a change.
- The understanding the User approved in this planning conversation, recorded in the Notes and design sections below.
- `.apm/archives/session-2026-11-15-001/spec.md`: session 1 decisions carried forward (palette, theme resolution, theme toggle), except the font.

Existing `AGENTS.md`: an `APM_RULES` block with Git, Local Testing, Scope and Reporting rules. Planning keeps that content and adds the test rule in the Notes. Existing `CLAUDE.md`: none. No import is needed.

## Notes

**Session boundary.** This session covers the Literata font, the letter reveal on headings, the section fade and rule draw, the Animations switch with its saved choice, reduced motion, and the theme fade and smooth scrolling. Later sessions cover "Doing now" and "Listening now" (session 3), the personal blog, the favicon and the writing voice.

**Changes from the brief and session 1.**
- Font: Literata replaces IBM Plex Sans across the site. The User chose this, which reverses session 1's font decision.
- Default: animations are on by default. The brief's reduced-motion rule still holds, so a visitor with no saved choice whose system asks for reduced motion starts in minimal mode.
- Headings: the letter reveal applies to the h1 as well as the section headings, as the User decided.
- Load: the User accepted the animation code on every visit. The page-weight rule is dropped. No new external resource is added beyond Google Fonts.
- Switch placement and navbar: after Task 1.2's review, the User moved the Animations switch from the navbar to the bottom right of the screen. It is fixed there, so it stays in view at every scroll position. The User also asked for the navbar to use the page background colour, not the surface colour, and for the navbar's shadow to go. The visible on/off control that the brief requires stays. The content restructure and the wider redesign go to a later session.

**Assumptions.**
- GitHub Pages serves `main`'s root as static files.
- Literata's Latin file is one variable font. Google's CSS points weights 400 and 500 at the same file, so the page downloads it once. The file is about 39 KB.
- Animation needs JavaScript. A visitor with JavaScript off sees plain headings, plain text and no switch.

**Test rule (added to `AGENTS.md`).** Animation work is checked in both modes, both themes, widths above and at 600px, and both reduced-motion settings.

## Typography

- Literata is the only typeface. Body text uses weight 400. Headings (h1, h2) and bold text use weight 500, the only two weights requested.
- Google Fonts link: `https://fonts.googleapis.com/css2?family=Literata:wght@400;500&display=swap`, with `rel="stylesheet"`. The IBM Plex Sans and IBM Plex Mono families are removed, because no element uses them.
- Font stack on `body`: `'Literata', Georgia, 'Times New Roman', serif`. Headings inherit it.
- Font sizes, line height and the 600px breakpoint stay as they are, unless a Task changes them for legibility and reports the change.

## Motion Modes and the Animations Switch

**Mode values.** The page is in one of two modes. The mode is set on `<html>` as `data-motion="animated"` or `data-motion="minimal"`.

**Resolution order.** An inline script in `<head>` sets the mode before first paint:
1. A saved choice in `localStorage` under the key `motion`, with value `animated` or `minimal`.
2. `prefers-reduced-motion: reduce` gives `minimal`.
3. Otherwise `animated`.

```js
// head script (sketch)
var saved = null;
try { saved = localStorage.getItem('motion'); } catch (e) {}
var mode = (saved === 'animated' || saved === 'minimal') ? saved
    : (window.matchMedia('(prefers-reduced-motion: reduce)').matches ? 'minimal' : 'animated');
document.documentElement.setAttribute('data-motion', mode);
```

Reduced motion is not a CSS media query, because a media query would override a saved choice. The `data-motion` attribute is the only switch.

**The switch.** A labelled control named "Animations", in a footer element, fixed at the bottom right of the screen so it stays in view.
- A `<button>` with `role="switch"`, and `aria-checked="true"` in animated mode.
- Reachable with Tab. Space or Enter changes it.
- Focus ring: 2px outline in `--focus`, offset 3px, as on the theme toggle.
- Appearance: the same track and knob as the theme toggle. The knob sits on the right when animations are on.
- Changing it sets `data-motion` at once and saves the choice under `motion`.
- Without JavaScript the switch stays hidden, since it cannot act. The script shows it.
- The theme choice keeps its own key, `theme`, and is unaffected.

## Mode Effects

| Effect | Animated | Minimal |
|---|---|---|
| Letter reveal on h1 and section h2s | On | Off. Plain text. |
| Section text fade | On | Off |
| Rule draw-in | On | Off. Rules show at once. |
| Theme colour fade (0.5s), and the toggle's track and knob, and the Animations switch's track and background | On | Off. Instant. |
| Nav link colour fade (0.3s) | On | Off |
| Smooth scrolling | On | Off. Jumps. |

In minimal mode every element has `transition: none`, and `html` has `scroll-behavior: auto`. The toggle and switch still work, but without motion.

## Letter Reveal

The reveal applies to the h1, which starts on page load, and to each section h2, which starts when it scrolls into view. Each heading plays once per activation of the mode.

**Each letter** starts at opacity 0, with an 8px blur and one full turn about the vertical axis (`rotateY(360deg)`). It transitions to opacity 1, no blur and no rotation over 0.6s with ease-out.

**Timing.** Letters start left to right. The delay between letters is `step = clamp(500ms / (letters − 1), 12ms, 45ms)`, so a heading finishes in about one second. The settle is the end of the last letter's transition. The font does not change.

**Words.** Each word is a group with `white-space: nowrap`, so a word never breaks across lines. Spaces remain text between the groups, so lines wrap only at spaces.

**Accessibility.** The full heading is kept once in a visually hidden element for screen readers. The split letters are `aria-hidden="true"`.

**Visibility.** A heading is hidden with `visibility: hidden` only after the script sets `data-motion="animated"`. Before that it shows as plain text.

**Trigger.** An `IntersectionObserver` with a threshold of 0.2 and a bottom root margin of −10%, so a heading starts just before it is fully visible. Each heading is unobserved once it starts.

**Performance.** Only `opacity`, `filter` and `transform` animate. No layout property is animated. The blur stays at 8px.

## Section Text and Rules

- **Section text** (the paragraphs and lists inside each `<section>`) fades from opacity 0 to 1 over 0.6s, once, when the section scrolls into view. It does not move.
- **Horizontal rules** (`hr`) draw in from the left. They scale on the X axis from 0 to 1 over 0.8s, once, when they come into view. The rule keeps its `--rule` colour and 1px height.
- The navbar, the contact icons and the toggles do not take these effects.

## Verification

- **Contrast:** `tools/contrast_check.py style.css`. The 16 pairs from session 1, 8 per theme, must pass. The font change does not affect colours.
- **Motion check (new, Task 3.1):** `tools/motion_check.mjs`, run in headless Chromium. It checks that animated mode is the default, that reduced motion starts minimal, that the switch changes the mode and saves it under `motion`, that minimal mode has no split letters, no transitions and no smooth scrolling, and that animated letters appear left to right and end in Literata.
- **Keyboard:** `tools/focus_check.mjs` extended in Task 3.1 to Tab to the Animations switch and check its focus ring.
- **Existing checks:** the theme check and the QA matrix from session 1, run in both modes.
- **Matrix:** each check runs in animated and minimal modes, at widths above 600px and at 600px or below, in both themes. The 390px check also confirms that the fixed switch causes no horizontal scroll, and that the navbar fits its links and the theme toggle.
- **User review:** the User reviews each Task on `http://localhost:8000` in both themes and both modes, and approves it before the Manager merges.
