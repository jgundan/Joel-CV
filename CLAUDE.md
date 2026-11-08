# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A static, single-page online CV (resume) for Joel Gundan, served through GitHub Pages. There is no build system, package manager, test suite, or linter.

Current state: the local `main` branch has no commits and the working tree is empty. The actual site (`index.html` and `.github/workflows/static.yml`) lives on `origin/main` (https://github.com/jgundan/Joel-CV). Run `git fetch origin` before editing, and check that the files are present.

## Commands

- Preview locally: open `index.html` in a browser. There is no dev server.
- Deploy: pushing to `main` runs `.github/workflows/static.yml`, which uploads the whole repo (`path: '.'`) as a Pages artifact and deploys it. `workflow_dispatch` also allows a manual run from the Actions tab.
- Tests and lint: none exist.

## Architecture

- **Single file.** All markup, CSS, and JS live in `index.html`. Keep it that way unless the user asks for a split.
- **Layout.** A GitLab-style profile page with no framework: a dark sticky top bar with a gradient rule, then a two-column grid (`.layout`) of bordered `.card` panels (with `.card-header` / `.card-body`) in the main column and a sticky `.sidebar` with contact info. The grid collapses to one column below 992px. Section ids are `about`, `experience`, `education`, `skills`, and `certifications`.
- **Navigation.** The top-bar `.topnav` links point at those section ids. A small inline script toggles `.active` with an IntersectionObserver. Keep the nav links and section ids in sync.
- **Theming.** All colors are CSS variables on `:root` (`--brand`, `--page-bg`, `--card-bg`, and so on), with dark-mode overrides under `prefers-color-scheme` and `[data-theme="dark"]`. Use the variables rather than hard-coded colors.
- **Print.** A `@media print` block flattens the layout for "Save as PDF", which is also the button in the profile card.
- **External dependencies, loaded from CDNs:** Font Awesome 6.4.2 (cdnjs) and Google Fonts (Inter). Nothing is installed locally, so the page needs network access to render correctly.
- **`about-me.md`** is a first-person profile in the repo root, written in Joel's voice and kept in sync with the CV. Update it alongside `index.html` when career facts change.
- **Content is the product.** Most edits are changes to resume text: experience, skills, certifications. Keep wording and dates consistent with the existing entries.
