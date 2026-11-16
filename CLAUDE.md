# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static, single-page online CV (resume) for Joel Gundan, served through GitHub Pages. There is no build system, package manager, test suite, or linter.

Work happens on feature branches (for example `redesign-resume`) and reaches `main` through a PR. The GitHub CLI is installed but may not be signed in. If `gh auth status` fails, give the user a compare link (`https://github.com/jgundan/Joel-CV/compare/main...<branch>?expand=1`) instead.

## Commands

- Preview locally: open `index.html` in a browser. There is no dev server.
- Deploy: merging to `main` runs `.github/workflows/static.yml`, which uploads the whole repo (`path: '.'`) as a Pages artifact and deploys it. `workflow_dispatch` also allows a manual run from the Actions tab.
- Tests and lint: none exist.

## Architecture

- **Single file.** All markup, CSS, and JS live in `index.html`. Keep it that way unless the user asks for a split.
- **Design.** It is modelled on the BootstrapMade "Style" template, rebuilt without Bootstrap. Fonts are Roboto (body), Quicksand (headings), and Open Sans (nav).
- **Theme.** The theme is black and orange. The page background is `#0b0b0b`, cards are `#1a1a1a`, and the accent is `#ff7a1a`. All colors are CSS variables on `:root`, with alpha variants such as `--accent-tint` and `--accent-30`. Text on orange fills uses `--on-accent` (near-black), because white on that orange fails contrast. The `@media print` block redefines the tokens for white paper, with a darker `#c2410c` accent. Keep hard-coded colors out of the screen styles.
- **Layout.**
  - A transparent fixed header gets a white background once scrolled (`.scrolled`).
  - A round toggle button opens a right-hand off-canvas menu (`.navmenu`, toggled by `body.nav-open`) at every screen width.
  - Sections in order:
    - `#hero`: split layout with stat counters, buttons, and the photo with floating tag cards.
    - `#about`: text, highlights, and a "My journey" timeline.
    - `#services`: the "What I Do" cards.
    - `#resume`: the `#experience` cards beside a side column holding `#skills`, `#certifications`, and `#education`.
    - `#contact`: contact cards. There is no form, because there is no backend.
- **Navigation.** The `.navmenu a.nav-link` hrefs must match section or block ids. The inline script does three things:
  - It sets `.active` on click and briefly ignores scroll updates.
  - It otherwise marks the section that most recently crossed 40% of the viewport height. Side-by-side blocks tie, and the first in nav order wins.
  - It forces the last link when the page is scrolled to the bottom.
- **Stats.** The final values are written in the HTML. The script counts up from 0 unless `prefers-reduced-motion` is set.
- **Profile photo.** The photo is embedded in `index.html` as a base64 `data:` URI, so it loads in any viewer. `profile.jpg` is the untracked source file. To change the photo, re-encode the image and replace the `src` of `.hero-photo`. Avoid line-based edits on that very long line.
- **Print ("Save as PDF").** A single `@media print` block lays the CV out on one A4 or Letter page:
  - It hides the header, menu, services, contact section, buttons, and decorations.
  - It shrinks the hero to the photo, name, and a print-only contact line.
  - It shows experience beside a 52mm side column.
  - To check the fit, render the page in an iframe 726px wide, with `@media print {` swapped for `@media all {`. The content must stay under about 986px tall, which is Letter with 9mm margins.
- **External dependencies, loaded from CDNs:** Font Awesome 6.4.2 (cdnjs) and Google Fonts. Nothing is installed locally, so the page needs network access to render correctly.
- **Other docs.** `README.md` lists accomplishments, and `about-me.md` is a first-person profile in Joel's voice. Keep both in sync with the CV when career facts change.
- **Content is the product.** Most edits are changes to resume text: experience, skills, certifications. Keep wording and dates consistent with the existing entries, and don't invent metrics such as skill percentages.
