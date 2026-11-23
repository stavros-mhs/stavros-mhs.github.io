# APM Spec

## Overview

This APM session adds the motion that session 002 paused to the personal site at https://stavros-mhs.github.io/. Headings reveal letter by letter, section text and rules appear once as they scroll into view, links and contact icons respond to hover, and the Animations switch turns all of it off and remembers the choice. The Literata font and the switch are already on `main`, so this session covers the motion itself. Success means every change passes the contrast, motion, focus and QA checks in both themes and both modes, and the User approves each Task on localhost.

## Workspace

Working repository: `/home/stavros-mhs/Projects/stavros-mhs.github.io`, the only repository in the workspace.

```text
stavros-mhs.github.io/
├── index.html    # single page, edited
├── style.css     # all styles, edited
├── cv.pdf        # unchanged
├── README.md     # unchanged
├── AGENTS.md     # Worker rules, local only, never committed
├── .apm/         # APM planning files, local only, never committed
├── .claude/      # APM skills and guides, local only, never committed
└── tools/        # local test tools, ignored by Git, never committed
```

Read-only references:
- `https://agentic-project-management.dev/`: the reference for the animation controls. Its home-page script respects `prefers-reduced-motion`. No on/off control was found in its main script, so the switch is designed for this site.
- `https://bigsky.software/cv/` and `https://joshblais.com/`: ideas only, not to copy.

Git:
- Base branch `main`, at `cc00cfc`, 25 commits ahead of `origin/main`. Nothing is pushed this session. GitHub Pages serves `main`'s root once the User pushes.
- One branch per Task, cut from `main`, named `type/short-description`, for example `feat/heading-reveal`.
- Commits use `type: description`, with type feat, fix, refactor, docs, test or chore. The Worker commits on its Task branch.
- The Manager merges into `main` only after the User approves the Task on localhost. Nothing is merged or pushed without the User's go.
- `.gitignore` lists `.apm/`, `.claude/`, `AGENTS.md` and `tools/`. Those stay local, and no Worker edits `.gitignore`.

Authoritative documents:
- This Spec's Notes and design sections, which record the understanding the User approved in the planning conversation.
- `.apm/archives/session-2026-11-15-001/brief-session-2.md`: the goals, must-haves and verification method for the motion work.
- `.apm/archives/session-2026-11-15-002/spec.md`: the motion design, carried forward where this Spec does not change it. The relevant sections are "Motion Modes and the Animations Switch", "Letter Reveal", "Section Text and Rules", "Mode Effects" and "Verification".

Existing `AGENTS.md`: an `APM_RULES` block with Git, Local Testing, Scope and Reporting rules. Its Local Testing rules already require animation work to be checked in both motion modes, both themes, widths above and at 600px, and both reduced-motion settings. Planning keeps that content and adds motion conventions inside the block. Existing `CLAUDE.md`: none.

## Notes

**Placed as Task 2.3 (User, 17 Nov).** The landing page becomes a terminal session. `whoami` prints `stavros-mhs`, `cat me.md` prints the intro paragraph, and an `ls` drops down the menu. The User calls it experimental. It changes the h1, the intro section and the Navbar section. Its details are agreed with the User before dispatch.

**Decisions of 17 Nov (User).** Reveals play all at once, with no queue, and the intro splits by sentence. The Projects list becomes a folder tree in a code block, hand-written, shown line by line inside `#projects`. Clicking a folder opens a short paragraph and a "source code here" link to the repository. Headings carry no `#`, and the trailing `/` marks a directory. The font is JetBrains Mono, decided. The loading still feels too quick, to revisit later. The phone switch and the listing text are accepted for now. The landing terminal is Task 2.3, and the phone pass is Task 1.5.

**Session boundary.** This session covers the heading reveal, section text, lists and rules, the hover states, the switch's hover ring and fade, the theme and switch fades, smooth scrolling, minimal mode, and the motion checks. "Doing now" and "Listening now" (session 3), the personal blog, the favicon, the writing voice, the content restructure and the wider redesign are later sessions.

**Already on `main`.** The Literata font; the Animations switch with its saved mode, fixed at the bottom right; the navbar on the page background, with no shadow. Contrast passes 16 of 16 pairs. Two accepted limits carry over: one anti-aliasing difference at light 390px (10 pixels, each one level), and a phone line length of 27 characters.

**Changes from the session 002 Spec.** These follow the User's choices in the planning conversation:
- The heading reveal fades each letter in and raises it 6px. The blur, the 360° spin and the perspective are removed. A spin on every visit gets tiring on a page people read, and a blur on every letter costs performance on phones.
- The Plex Mono start for headings is dropped. It would need a second font, and letters would jump in width. The font stays Literata throughout.
- Skills and Projects list items appear one after another.
- Reveals play one at a time, top to bottom, instead of each starting on its own. The items on the first screen play at load, and the rest play as they scroll into view.
- Headings wait at `opacity: 0` instead of `visibility: hidden`, so screen readers keep them in the heading list.
- Hover states are added: link underlines, contact icon lift, and the switch's hover ring.
- The switch's background and track fade with the theme in animated mode.

**Decisions for the Manager to keep in view.**
- The accent colour stays on headings and icons, as session 1 set it. The switch's ring uses `--link`, the switch's own text colour, which already passes 4.5:1 as text on both backgrounds.
- The new link underline replaces the current `a:hover` and `a:active` underline, which appears at once.
- Text placed directly in a section, the intro and the Hobbies line, has no element to animate. It is wrapped in an element. The text itself does not change.
- The h1 reveal is predicted at about 1.13 seconds. Its 45 letters, not counting spaces, reach the 12ms minimum step. The Worker reports the measured time and keeps the minimum unless asked.
- The `tools/contrast_check.py` pairs need a pair for the switch's ring if `--link` on `--bg` is not already listed. The README says to add one for any new focus or non-text colour.
- **Roll order and screen readers (User, approved).** Reveals play top to bottom, one at a time, and the first-screen items play at load. Headings wait at `opacity: 0`, not `visibility: hidden`, so screen readers keep them in the heading list. Task 1.2 builds the queue and this change.

**Assumptions.**
- GitHub Pages serves `main` as static files.
- Animation needs JavaScript. Without it, the page shows plain headings and text, and no switch.
- Playwright's `reducedMotion` option emulates both "reduce" and "no preference", so both states are checked in Playwright. Task 1.1 used it in 24 matrix cells.
- Hover checks depend on whether headless Chromium reports `(hover: hover)`. If it does not, the Worker reports those cells as skipped, with the reason.

**Work left for later sessions.** "Doing now" and "Listening now", the personal blog, the favicon, the writing voice, the content restructure and the wider redesign.

## Terminal Landing

Set on 19 Nov, on the User's vision. It replaces the Navbar section, the Projects Tree section and the section reveals for the landing and the content pages. The Animations switch, the palette, the font and the hover rules still apply.

- **Terminal.** A terminal sits in the centre of the landing page, above the content. It is at most 760px wide, and it spans the viewport with a 16px gutter at 600px or below. It uses the code-style surface, 6px corners, and the palette tokens only.
- **Prompt.** `~$` at the top. Below a `cd`, the prompt shows the path, such as `~/projects/aralloc$`.
- **Landing commands.** The terminal types `whoami`, which prints `stavros-mhs`. It types `ls -l`, which prints the top level as tree lines, ending in a count line:

```
~/
├── user/
├── projects/
├── skills/
└── contact/

4 directories
```

- **Directories and files.** The full tree. Clicking a name prints the listing one level down. Every listing ends with its count line, as the Unix `tree` command does.

```
~/
├── user/
│   ├── HOBBIES.md
│   └── WHOAMI.md
├── projects/
│   ├── aralloc/
│   │   ├── BRIEF.md
│   │   └── source/
│   └── ginny/
│       ├── BRIEF.md
│       └── source/
├── skills/
│   └── SKILLS.md
└── contact/
    └── INFO.md
```

- **Navigation (amended by the User on 20 Nov).** Clicking a folder in the tree opens or closes it in place, and its rows appear under it. Its count line changes as rows appear or close, and the click makes it the current path. The navbar, the back and forward buttons, and a typed address open a folder page: the terminal sits at the top, types `tree <folder>/`, prints the tree, then shows `cd ..` and a prompt with the path. Clicking a `.md` file opens a file page: the terminal types `cat <name>` at the folder's prompt, then shows `cd ..` and a new prompt, and the file's markdown prints below the terminal. `cd ..` goes up one level, or to the landing from a top-level folder. `source/` is highlighted as a link, and it opens the GitHub repository in a new tab. The command history is gone: the terminal shows one page at a time.
- **Address bar.** The path follows the state after `#`, for example `#/projects/aralloc/`. The browser's back button follows it too.
- **Navbar.** After the landing output, the navbar drops down from the top of the screen: `user/`, `projects/`, `skills/` and `contact/`, in that order. Hobbies sit under `user/`, not in the navbar. On phones, a burger in the navbar opens the same top-level tree lines as a list.
- **Motion, animated.** The terminal fades in over 500 ms. `whoami` types one character every 60 ms, then pauses 300 ms, then prints. `ls -l` does the same, and its output lines print 120 ms apart, each fading in over 250 ms. The navbar drops over 500 ms, ease-out. A cursor blink, if used, changes opacity only.
- **Minimal mode (set on 19 Nov, built later).** No terminal and no animations. The page is a scroll from top to bottom. Each section is a header, which is the file's path, and the content of its `.md` file, in the order `user/WHOAMI.md`, `user/HOBBIES.md`, `projects/aralloc/BRIEF.md`, `projects/ginny/BRIEF.md`, `skills/SKILLS.md`, `contact/INFO.md`.
- **Mobile (19 Nov).** Not finalised. For now the page must not render badly at 390px. The detailed mobile pass follows the desktop product. Its details are not in this Spec yet.
- **Without JavaScript.** The landing shows its commands and output as static text, and the navbar shows as links. Each `.md` file is a link to the raw file.
- **Content.** The text lives in real files in the repository: `user/WHOAMI.md`, `user/HOBBIES.md`, `skills/SKILLS.md`, `contact/INFO.md`, and `projects/<name>/BRIEF.md`. The `source/` links point to GitHub, not to a folder.
- **Markdown.** The subpage shows the markdown as rendered text for the subset used: headings, paragraphs, lists, links, bold and code. This is a default for the User to confirm.

## Navbar (superseded 19 Nov, see Terminal Landing)

The navbar is a row of directory names, styled as the output of `ls`. The desktop row and the phone menu show the same four directories. (User, 17 Nov.)

- **Names.** `projects/`, `skills/`, `hobbies/` and `contact/`. Each links to its section, with the same hrefs as now.
- **Headings.** Headings carry no `#`. The trailing `/` marks a directory. The `#` was dropped because it clashed with the `/`.
- **Desktop (above 600px).** The four names sit on one line, separated by spaces, like `ls` output. They appear at once in both modes.
- **Phone (600px or below).** A burger button sits in the navbar. It opens a panel with the `ls -l` listing: one directory per line, each a link, with the fixed columns `drwxr-xr-x  2 guest guest 4096 Nov 17` before the name. In animated mode the command types letter by letter, then the lines fade in, 80 ms apart. Minimal mode shows everything at once. The burger opens and closes the panel, and Escape closes it.
- **Without JavaScript.** The four names show in the navbar as links, with no burger and no panel.
- **Focus.** The burger's focus ring is 2px solid `--focus`, offset 3px.

## Projects Tree (withdrawn 19 Nov, see Terminal Landing)

The Projects list becomes a folder tree in one code block, inside `#projects`. It is the main aesthetic of the site (User, 17 Nov).

- **Tree.** A `projects/` root line, then one folder per project, each with a one-line description in the block. Folder names are buttons. Proposed text, pending the User's confirmation: `ginny/` with "homework solver, thesis", and `aralloc/` with "arena allocator, C/C++".
- **Panel.** Clicking a folder opens one panel below the block. It holds a short paragraph of a few lines about the project, and a link, "source code here", to its repository. The User writes the paragraphs. Clicking the open folder again closes the panel. Clicking another folder replaces it.
- **Repository links.** The existing repository URLs are kept. Without JavaScript, the folder names link to their repositories, and no panel appears.
- **Source.** Written by hand in `index.html`. No script runs, and no request goes to GitHub when the page loads.
- **Style.** The block uses the surface colour and 6px corners, like the code-style headings. Its text, folder names and link pass 4.5:1 in both themes. The focus ring is 2px solid `--focus`, offset 3px, as on the switch.
- **Motion.** In animated mode, each tree line fades in from opacity 0 to 1 over 250 ms, starting 80 ms after the line above. The block plays once, when its section scrolls in, with the observer settings from Heading Reveal, Trigger. The panel fades in over 250 ms. Lines and panels are hidden only under `html[data-motion="animated"]`, until they play.
- **Minimal mode.** Every line shows at once, and the panel opens at once.
- **Width.** At 390px, the widest line fits inside the block, and the page does not scroll sideways. A line that does not fit is shortened, not wrapped.

## Typography

Carried forward, and already on `main`.
- Decided (User, 17 Nov): JetBrains Mono is the site's only typeface. It replaces Literata.
- Body text uses weight 400. Headings (h1, h2) and `strong` use weight 500.
- Google Fonts link: `https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500&display=swap`, with `rel="stylesheet"`. No IBM Plex link remains.
- Body font stack: `'JetBrains Mono', ui-monospace, Menlo, Consolas, monospace`.
- Font sizes, line height and the 600px breakpoint stay as they are.

## Motion Modes and the Animations Switch

**Mode.** The page is in one of two modes, set on `<html>` as `data-motion="animated"` or `data-motion="minimal"`. The head script, already on `main`, resolves the mode before first paint, in this order:
1. A saved choice in `localStorage` under the key `motion`, with value `animated` or `minimal`.
2. `prefers-reduced-motion: reduce` gives `minimal`.
3. Otherwise `animated`.

Reduced motion is not a CSS media query, because a media query would override a saved choice. The `data-motion` attribute is the only switch.

**The switch.** A `<button role="switch">` labelled "Animations", in the footer and fixed at the bottom right: 1rem from the right and bottom edges, and 0.5rem at 600px or below.
- It is hidden in the markup, and the script shows it.
- `aria-checked="true"` in animated mode.
- Tab reaches it. Space and Enter change it.
- Its focus ring is 2px solid `--focus`, offset 3px, as on the theme toggle.
- Its track and knob match the theme toggle, with the knob on the right when animations are on.
- Changing it sets `data-motion` at once and saves the choice under `motion`. The theme key `theme` is unaffected.

**Switch hover ring.** In animated mode, on a device with hover, a 1px ring in `--link` fades in over 0.3s around the switch, so its shape shows. In minimal mode there is no ring on hover.

**Switch fade.** In animated mode, the switch's background and track change colour over 0.5s with the theme. In minimal mode, the change is instant.

**Changes during a visit.** The reveals are set up at page load, when the mode is animated. Switching during a visit changes transitions, hover states, smooth scrolling and the switch's fade at once. It does not split headings or replay anything on the current page. Switching to minimal shows any content not yet revealed at once. Switching to animated applies the reveals from the next page load.

**Without JavaScript.** The switch stays hidden. The page shows plain headings and text, with no motion.

## Heading Reveal (retired on 19 Nov by the User; it can be added back later)

**Scope.** The h1 and each h2, plus the section text, list items and rules in the Section Text, Lists and Rules section. Every reveal plays in one queue, in page order, one at a time.

**Split.** The split runs only when `data-motion` is `animated`, after `document.fonts.ready`, so letter positions are measured in Literata.
- Each heading keeps its full text once in a visually hidden element, for screen readers.
- The visible letters sit in an `aria-hidden="true"` wrapper, one `.word` group per word, with `white-space: nowrap` on each group and spaces between the groups, so lines wrap only at spaces.
- Each letter is a `display: inline-block` span, so transforms apply to it.
- The em dash in the h1 counts as a word.

**Motion.** Each letter starts at opacity 0 and 6px below its place. It moves to opacity 1 and no offset, with ease-out. Timings on the branch at 17 Nov, to be touched up later: the heading takes 500 ms, and each letter fades over 250 ms.
- Letters start left to right. The starts are spread across the heading's 500 ms, so the letters run left to right.
- The settle time is the end of the last letter's transition.
- The font does not change during the reveal.

**Visibility.** In animated mode, a heading is at `opacity: 0` until its reveal starts, then at opacity 1. Opacity keeps the heading in the accessibility tree, so screen readers still list it. If the split fails, the script shows the heading as plain text.

**Trigger.** All headings start together at page load, once the font is ready and the split has run. Each section's text, list items and rules start together when that section scrolls into view, using an `IntersectionObserver` with threshold 0.2 and `rootMargin` `0px 0px -10% 0px`. Each section plays once and does not replay. Nothing waits for another item. The User removed the queue on 17 Nov.

**Minimal mode.** No split. Headings show as plain text.

## Section Text, Lists and Rules

**Section text.** Everything in a section except its h2 and its list items: paragraphs, the intro, the Hobbies line and the Projects tree. It fades from opacity 0 to 1 over 250 ms with ease-out, once, when its section scrolls in (see Heading Reveal, Trigger). The intro is split by sentence, and each sentence starts 125 ms after the one before. It does not move.

**Lists.** The Skills list items fade in the same way as section text, one after another, with their starts spread across the section's 500 ms. Skills has four items. The Projects list is replaced by the Projects tree (see Projects Tree). Lists do not fade as a whole.

**Rules.** Each `hr` draws in from the left. It scales on the X axis from 0 to 1 over 500 ms with ease-out, once, with the content of its section as that section scrolls in. The rule is a 1px element in `--rule`, with the same margin as now, so the layout does not shift. The current `border-top` changes to a 1px background so the rule can scale.

**Hidden states.** Section text, list items and rules start hidden only under `html[data-motion="animated"]`, and only until their queue step plays. With JavaScript off, or in minimal mode, they show at once.

**Not animated.** The navbar and the theme toggle.

## Hover States

These apply only on devices with hover, through `@media (hover: hover)`, so phones do not keep a stuck hover state.

- **Link underlines.** Every text link on the page and in the navbar. The underline is a 1px line at the text baseline, in the link's own colour (`currentColor`), drawn with `::after` and `transform: scaleX(0)`. On hover it scales to 1 from the left over 0.3s, ease-out. It replaces the current `text-decoration` underline on `a:hover` and `a:active`. In minimal mode it appears at once.
- **Nav link colour.** Keeps the 0.3s colour change in animated mode. In minimal mode the change is instant.
- **Contact icons.** When a contact row is hovered, its icon moves up 2px over 0.3s, ease-out, in animated mode. In minimal mode it moves at once. The icon keeps its colour. The current rule that sets `transition: none` on contact icons is overridden in animated mode only.
- **Switch.** See Motion Modes and the Animations Switch.

## Theme Transitions and Smooth Scrolling

- **Kept in animated mode:** the body background and text colour (0.5s), the navbar background (0.5s), the theme toggle's track and knob (0.5s), and link colours (0.3s).
- **Smooth scrolling:** `html` has `scroll-behavior: smooth` in animated mode only. In minimal mode it is `auto`, so the page jumps.
- **Minimal mode:** every element, and its `::before` and `::after`, has `transition: none !important` and `animation: none !important`. This overrides the existing `transition: all` rules on the toggle circle and the icons.
- **Animation properties:** the new effects animate only opacity and transform. The existing colour and toggle transitions stay as they are. The one exception is the Animations switch's background, track and knob, which fade in colour over 0.5s in animated mode, to match the theme toggle.

## Mode Effects

| Effect | Animated | Minimal |
|---|---|---|
| Heading letters (h1, h2) | Fade and rise 6px, left to right, all headings together at page load | Plain heading |
| Section text | Fades in once, when its section scrolls in | Shows at once |
| Skills items and the Projects tree | Skills items fade one after another. Tree lines print one after another, 80 ms apart. Opening a folder fades in its panel | Show at once |
| Rules | Draw in from the left, once | Show at once |
| Link underline | Grows from the left on hover, 0.3s | Appears at once |
| Nav link colour | 0.3s fade | Instant |
| Contact icon | Lifts 2px on row hover, 0.3s | Moves at once |
| Switch hover ring | Fades in on hover, 0.3s | No ring |
| Theme colour, toggle, switch fade | 0.5s | Instant |
| Smooth scrolling | On | Off, the page jumps |

## Verification

- **Contrast:** `python3 -B tools/contrast_check.py style.css` passes 16 of 16 pairs, in both themes. The new underline and ring use `currentColor` and `--link`, which are already in the palette. If the ring's pair is missing from the script, the Worker adds it.
- **Motion check (new):** `tools/motion_check.mjs`, run in headless Chromium. It checks that animated is the default; that reduced motion starts minimal; that the switch changes the mode and saves `motion`; that minimal has no split letters, no transitions and no smooth scrolling; that animated letters appear left to right and end in Literata at opacity 1 with no offset; that the reveals play one at a time in page order, with the first-screen items at load; that all five headings are in the accessibility tree before scrolling in animated mode; and that the switch ring, the link underlines and the contact icon lift appear on hover in animated mode only.
- **Focus check:** `tools/focus_check.mjs` is extended to Tab to the switch and check its focus ring in both themes.
- **Existing checks:** the theme check and the QA matrix from session 1, run in both modes.
- **Matrix:** each check runs in animated and minimal modes, at 1280px and 390px, in both themes. At 390px the fixed switch causes no horizontal scroll, and the page logs no console errors.
- **Measured timing:** the Worker reports the measured duration of the h1 reveal and of one h2 reveal, and the fade and rule durations.
- **User review:** the User reviews each Task on `http://localhost:8000` in both themes and both modes, and approves it before the Manager merges.
