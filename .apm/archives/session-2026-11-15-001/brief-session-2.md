# Session 2 Brief: Animations and the Minimal Mode

Read this file as initial context. Ask your question rounds before writing the Spec, Plan or Rules, following the APM planning process.

## Where the site is now

Session 1 is complete and merged into `main`, at `ca642de`. Nothing is pushed to GitHub yet. The User will push once all sessions are finished.

What session 1 delivered:
- **Font:** IBM Plex Sans throughout. The User compared it with IBM Plex Mono and Literata, and chose Plex Sans.
- **Light theme:** a cream paper palette, with orange headings, brown links and a toggle that shows the mode you would switch to.
- **Dark theme:** unchanged. Orange and dark grey.
- **Theme:** set before first paint, from a saved choice, then the system setting, then dark.
- **Toggle:** keyboard-reachable, with a visible focus ring.
- **Local test tools:** in `tools/`. Git ignores this folder, so it is never committed or pushed. Read `tools/README.md` before any browser or contrast check.

The Spec, Plan and Rules of session 1 are in `.apm/`. They are the reference for the decisions above.

## Goal

Add some minimal, tasteful animation to the site, so it has a little more personality. Add a minimal mode that turns the animations off. The mode must be easy to reach, and it must default to honouring the visitor's reduced-motion setting.

The User's own idea for the headers: each heading loads from left to right. It starts in IBM Plex Mono and changes to Literata, not to Plex Sans. Plex Sans stays the body font. This is the User's preferred direction for the header animation, but the Planner should check it against the rest of the design.

## Reference

- **APM site:** https://agentic-project-management.dev/ is the official APM site, and the User confirmed it. It is the reference for the animation controls. Look at how it presents its animations, and how it lets a visitor turn them off.
- **Ideas only, not to copy:** https://bigsky.software/cv/ and https://joshblais.com/.

## Must-haves

1. **Minimal animations.** A small set. Propose them, with reasons. The header animation above is one. The Planner should propose others, and keep them subtle.
2. **Minimal mode.** A visible control that turns animations off, and turns them back on. Its position and form are for the Planner to propose, for example next to the theme toggle. It must work with the keyboard, and it must have a visible focus ring.
3. **Reduced motion.** With no saved choice, the site follows `prefers-reduced-motion`. A saved choice overrides it. Store the choice in `localStorage`, under a key the Planner proposes.
4. **Theme transitions.** Session 1 left the 0.5s theme transitions in place. Session 2 owns them. The Planner decides whether they stay, shorten, or switch off in minimal mode.
5. **Smooth scrolling.** Session 1 left `scroll-behavior: smooth` in place. Session 2 owns it too, and the same rules apply.

## Stack

- Plain HTML, CSS and vanilla JavaScript, with no build step and no framework. Prefer CSS animations, which need no JavaScript. Use JavaScript only where the minimal mode control needs it.
- Any framework or animation library needs the User's approval, justified against technical debt.
- The only external resource is Google Fonts. If the header animation needs IBM Plex Mono and Literata, they are loaded from Google Fonts. Say so in the Spec, because it adds to that rule.

## Constraints to respect

- Keep the palette, the toggle's appearance, the focus ring, and the contrast results from session 1.
- The contrast check in `tools/` must still pass.
- Headings use weight 500, because only the regular weight of Plex Sans is loaded. Check the header animation with that in mind.
- The site is hosted on GitHub Pages. Assume a plain static file server.

## Out of scope

- **"Doing now" and "Listening now"** sections: session 3.
- **The personal blog:** a later session.
- **Favicon:** an idea for later, not part of session 2.
- **The writing voice:** content, not design.
- **Pushing to GitHub:** after all sessions are finished.

## Verification

- Use the test tools in `tools/`, as documented in `tools/README.md`: the theme check, the keyboard check, the QA matrix and the contrast check.
- A motion check does not exist yet. The Planner should propose one, to confirm that the animations switch off with minimal mode, and with the reduced-motion setting on. It would go in `tools/`.
- The User reviews the look and feel on localhost, in both themes, with the animations on and off.

## Working method

- One branch per Task, cut from `main`. The Worker commits. The Manager merges only after the User approves.
- Nothing is pushed.
- Review each branch on localhost. The User gives specific feedback.

## Questions for the User

- Which animations do you want, beyond the header idea?
- Where should the minimal mode control sit, and what should it look like?
- How long should the header animation take?
- Should the minimal mode remember its choice across visits, as the theme does?
