# Site Refinement Brief

Read this file as initial context. Ask your question rounds before writing the Spec, Plan or Rules, following the APM planning process.

## Goal

Refine my personal site, a static GitHub Pages site at https://stavros-mhs.github.io/. Keep it minimal, but give it more personality through smoother color use, and add two small sections that I can update by hand: "Doing now" and "Listening now".

## References

- https://bigsky.software/cv/: minimal CV layout.
- https://joshblais.com/: personality through color and type.
- https://agentic-project-management.dev/: a friend's site, more elaborate. Use it for ideas only. It is also the APM project's own site, and it is the reference for the animation controls.

## Must have

1. **Calmer color usage.** Right now `--orange-accent` is applied to every h1, every h2 and the contact icons. Define a palette with fewer accent roles, consistent in both themes.
2. **Dark and light mode.** The toggle exists. Respect `prefers-color-scheme` on the first visit, prevent a flash of the wrong theme before JavaScript runs, and keep the saved choice.
3. **Minimal mode that disables animations.** Model it on the animation controls on agentic-project-management.dev. Default to honoring `prefers-reduced-motion`, and let me override it with a control on the page.
4. **"Doing now" section.** Shows the project or hobby I am working on right now, as a short line with an optional link. I must be able to update it by editing one data file. No rebuild, no code changes.
5. **"Listening now" section.** Shows the album I am listening to: title, artist, cover image and an optional link. I must be able to change it by editing the same data file and replacing one image file in the repo. Live integration with Spotify or any other service is out of scope unless I ask for it later.

## Stack

- Plain HTML, CSS and vanilla JavaScript. No framework, no build step, no bundler, no package manager.
- No new runtime dependencies. The existing Google Fonts link is the only external resource.
- Changes stay within the existing files plus the new data file and cover image folder. Add anything else only if you justify it and I approve it.
- The data file must work when the site is opened directly from disk or served locally, so do not rely on `fetch()`. A plain JavaScript data file loaded with a `<script>` tag is the expected approach, but you may propose an alternative with reasons.

## Current state (from audit)

- `index.html` is a single page with Projects, Skills, Hobbies and Contact sections. The theme toggle is a checkbox plus `localStorage`. The theme is set by a script at the end of `<body>`.
- Dark is the default through `body class="dark-mode"`. The dark palette is defined twice, in `:root` and in `body.dark-mode`.
- Light mode uses `body:not(.dark-mode)` overrides.
- Motion today: 0.5s transitions on background and color, 0.5s toggle animations, `transition: all` on the SVGs and toggle circle, and `scroll-behavior: smooth`. Nothing checks `prefers-reduced-motion`.

Bugs to fix:
- The Google Fonts `<link>` has no `rel="stylesheet"`, so IBM Plex Sans never loads.
- The CSS asks for `'Inter'`, so the page falls back to Roboto or Arial.
- The CSS header says "Catppuccin Frappé", but the palette is custom.
- `--base: #2e2e2e;;` has a doubled semicolon.
- Because the theme script runs at the end of `<body>`, light-preference visitors see a dark flash.

## Branches, testing and pushing

- Create one branch per Task, from `main`. Use descriptive names, for example `fix/font-loading` or `feat/now-section`. Each branch is verified locally before it is considered done.
- Test everything on localhost before a branch is considered done. Serving the repo root with `python3 -m http.server` is enough. Check both themes, both motion settings, the 600px breakpoint and contrast.
- Propose any automated checks in the Plan, keep them minimal, and get my approval before adding them.
- Nothing is pushed to any remote until I say so.
- Nothing is merged into `main` until I say so.
- Workers and the Manager must follow these rules, so put them in the Rules document.

## Out of scope

- Backend, CMS, analytics, comments, blog.
- Live music or activity integrations.
- Changing CV content or `cv.pdf`.
- Changing existing project or contact content, unless a task explicitly calls for it.

## Suggested stages

The planner may adjust these.

0. **Setup:** confirm branch naming, local test method and the no-push rule in the Rules document.
1. **Foundations:** font loading, theme flash, duplicated variables, CSS header.
2. **Color system:** tokenized palette with fewer accent roles, checked in both themes.
3. **Motion:** `prefers-reduced-motion` support, plus a minimal-mode control that disables transitions and smooth scrolling.
4. **Now and Listening sections:** data file, cover image handling, layout and placement, and an optional nav link.
5. **QA:** local checks on mobile, both themes, both motion settings and contrast.

## Questions for me

- Keep orange as the single accent, or choose a new one?
- Keep IBM Plex Sans (the loaded font), or switch to Inter (the CSS font)?
- Is minimal mode a separate control, or part of the theme toggle?
- Where should "Doing now" and "Listening now" go on the page, and should they appear in the nav?
- What image size and format should the album covers use? Do I supply the cover files myself?
- Should the cover image be a local file, or a URL to an image I host elsewhere?
- Keep the current content and section structure, or change it?
