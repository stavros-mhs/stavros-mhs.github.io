APM_RULES {

## Git

- Work only on the branch your Task Prompt names. Do not switch branches unless the Task Prompt says so.
- If the Task Prompt says to create the branch, create it from `main`. Otherwise it must already exist. If it does not, stop and report.
- Commit at logical points, and once the Task's steps are complete. Commit messages use `type: description`, where type is one of feat, fix, refactor, docs, test or chore. Keep the description lowercase, imperative and under 72 characters.
- Commit messages carry no trailer lines, such as `Co-Authored-By`. The User asked for these to stay out of the history.
- Before committing, run `git status` and stage only the files your Task changes.
- `AGENTS.md`, `.apm/`, `.claude/` and `tools/` are local-only, except that the `apm` branch carries `.apm/`, `.claude/` and `AGENTS.md` to the User's other machine. Only the Manager updates that branch, and only when the User asks. On every other branch, never stage, commit or push these files.
- Never run `git push`, merge into `main`, rebase, force-push or delete a branch. The User decides these.

## Local Testing

- Serve the site from the repository root with `python3 -m http.server`. Check it at `http://localhost:8000`. If port 8000 already serves the repository root, use that server, and never start a second server on the same port.
- Test every change in both themes. Test layout at widths above 600px and at 600px or below.
- Before reporting any colour change, or any new focus or non-text indicator, run `python3 -B tools/contrast_check.py style.css`. Normal text must reach a contrast ratio of 4.5:1 in both themes, except the terminal's directory names in light mode (`--dir` and `--dir-hover` under `:root[data-theme="light"]`), which must reach the agreed 3.31:1, as `tools/contrast_check.py` sets it. Large text, which is headings at 24px and above (h1 and h2), must reach 3:1, as in WCAG AA. Focus rings and other non-text indicators, such as icons, must reach 3:1. See `tools/README.md` for the other test tools.
- Do not leave debug files, screenshots or temporary pages in the repository unless your Task Prompt asks for them. Write screenshots to the scratchpad directory.

## Motion

- Test animation work in both motion modes, animated and minimal, with reduced motion both on and off, in both themes, at widths above 600px and at 600px or below.
- Animate only `opacity` and `transform` in any new effect. Do not animate `filter`, `width`, `height`, `top`, `left`, `margin` or font properties in new effects. Existing colour and toggle transitions stay as they are. The Animations switch's background, track and knob may fade in colour over 0.5s, in animated mode only, to match the theme toggle's existing colour transitions.
- Use the `data-motion` attribute on `<html>` as the only switch between modes. Do not use `prefers-reduced-motion` in CSS.
- Hide content for a reveal only under `html[data-motion="animated"]`, and only until its observer has fired. With JavaScript off, or in minimal mode, the content shows at once.
- Put hover effects inside `@media (hover: hover)`, so touch devices do not keep a stuck hover state.
- Every new transition or animation must respect the minimal-mode rules. Do not write a rule that re-enables a transition in minimal mode.
- When you report a duration, position or other measured value, give the number and how you sampled it.
- Full-page screenshot comparisons are noisy. Compare the same element, or repeat each capture, before you report a difference.

## Scope

- Edit only the files your Task Prompt names. If a change needs another file, stop and report it.
- Keep the site as plain HTML, CSS and vanilla JavaScript. Do not add frameworks, build steps, package managers, animation libraries or external resources beyond what your Task Prompt names.
- Do not change text, links, `cv.pdf` or other content unless your Task Prompt says so. Wrapping text in an element is allowed only when the Task Prompt asks for it, and the text itself must not change.
- Do not add, remove or change animations or transitions unless your Task Prompt says so.

## Reporting and Review

- Report what you changed, the checks you ran and their results. If a check failed, say so and give the observed behaviour.
- Do not claim a check passed unless you ran it. If a check cannot run here, report it as skipped and give the reason.
- Name the branch and the local URL the User should refresh.
- When your Task Prompt names a User action, stop there and wait for the User. Do not make the User's choice for them.

} //APM_RULES
