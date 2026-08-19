# CLAUDE.md

Guidance for working on this repo — a static resume site deployed via GitHub Pages.

## What this repo is

- `index.html` — the live resume, single source of truth for content, served as-is (no build step).
- `css/style.css` — shared stylesheet for `index.html` and `draft.html`.
- `draft.html` — a sandbox copy of `index.html`, used to try layout/CSS changes before they land.
- `resume.tex` — a LaTeX/Overleaf version of the same resume content, kept in sync with `index.html`.
- No `package.json`, no JS, no bundler. Keep it that way — don't introduce a build step for a static one-page site.

## Sync rule: `index.html` and `resume.tex` change together

Any content edit to `index.html` — wording, bullets, dates, links, section order, headline — must be mirrored into `resume.tex` in the same change. `resume.tex` is the user's actual submitted-to-ATS artifact in some workflows, so letting it drift out of sync silently produces an inconsistent resume across channels. Commit both files together so they can't drift apart unnoticed.

## Experimentation workflow: use `draft.html`

Prototype layout, CSS, or structural changes in `draft.html` first. Verify visually and via Print Preview. Only port a change into `index.html` once it's confirmed working. After porting, re-sync `draft.html` to match `index.html` — it should not be left permanently diverged; it's a workbench, not a second live version.

## Print / ATS constraints (do not re-litigate these)

- The printed/PDF output must stay on **one page**, single column, top-to-bottom reading order — regardless of the two-column layout used on screen. The `@media print` block in `index.html`'s inline `<style>` owns this; it deliberately overrides the on-screen flex layout.
- Never reformat Open Source contributions (or any project) as fake employment entries (fabricated "Company | Title | dates" framing) to game ATS keyword/experience parsing. Open Source and Projects sections stay honestly labeled as what they are.
- Contact info: on screen, icons only; in print, plain text only (icons are hidden in print — they cost vertical space and add nothing for ATS parsing or a human reading a printed page).

## Code style for new/edited HTML & CSS

- Use current HTML5 semantics; avoid deprecated attributes/patterns.
- Prefer modern CSS (flexbox/grid, custom properties) over legacy hacks.
- Avoid `!important` unless there's no other way to win a specificity fight against an existing rule (this repo already leans on it for responsive breakpoints and print overrides) — when you do use it, it should be obvious from context why (e.g. inside a `@media` override block).
- Keep the print stylesheet changes scoped to `@media print` blocks; don't let print-only tuning bleed into the on-screen styles.

## Verification before committing

- No automated PDF/page-count renderer is available in this dev environment — always verify print-affecting changes with an actual browser Print Preview before committing.
- Check both the on-screen two-column layout and the printed single-column, single-page output after any layout/CSS change.
- If `resume.tex` was touched, compile it (Overleaf, or `pdflatex resume.tex` locally) to confirm it still fits one page and has no syntax errors.
