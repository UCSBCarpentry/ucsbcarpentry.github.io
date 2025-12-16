## Purpose

Short, repository-specific guidance to help an automated coding agent be immediately productive editing this Jekyll-based GitHub Pages site.

## Quick summary

- This repository is a Jekyll site (source -> `_site` build output). Primary content lives in root pages and `_posts/`.
- Build locally with Bundler + Jekyll. GitHub Pages publishes the site from the repository (no additional CI files found).

## Local dev / build commands (Windows cmd.exe)

1. Install Ruby (>= 2.7.4) and Bundler.
2. From the repo root:

```
bundle install
bundle exec jekyll serve --livereload --future
```

Notes: the `Gemfile` includes `github-pages` so local builds should use Bundler to match GitHub Pages plugin versions. `_config.yml` sets `future: true` so posts dated in the future will be included when serving locally if `--future` or `future: true` is present.

## Key files & patterns (actionable)

- `_posts/` — blog-like content. Filenames use the Jekyll convention `YYYY-MM-DD-title.md`. Use existing files as examples for front-matter structure (layout, title, tags).
- `_layouts/` — HTML templates used by pages and posts. Example files: `_layouts/workshop.html`, `_layouts/meeting.html`. If you need to change overall page chrome, prefer editing or adding a layout here.
- `index.md`, `past-workshops.md`, `community/` — top-level pages. Update these for site navigation/content changes.
- `css/ucsb.css` and `css/simple.css` — site styles. New styles should be added to these files or `assets` per existing pattern.
- `_site/` — generated output. Do NOT edit files here; change the source files and rebuild.

Project-specific gotchas discovered in the repo
- There was a file named `_layouts/default.hmtl` (misspelling). It has been corrected to `_layouts/default.html`. If you touch layouts, verify the filename extensions.
- The repo uses `future: true` in `_config.yml` — future-dated posts may appear locally and in builds unless you change this.

## How to make safe edits (examples)

- Change a community meeting page: edit `_posts/YYYY-MM-DD-community-meeting.md` and update front matter `layout: meeting` if needed. Then run the local server and verify the page under `/meeting/`.
- To modify styles for workshop pages, edit `css/ucsb.css` and refresh the served site.

## Debugging hints

- If `bundle exec jekyll serve` fails, run with `--verbose` to see plugin/load errors.
- Check `_site/` after a successful build to confirm generated HTML and asset paths.
- Search for layout usage: posts include a `layout:` front matter — match that to files in `_layouts/`.

## Conventions for PRs and branches

- This repository appears to use branch `dev-instructors` for in-progress work (current branch when these instructions were written). Confirm the repository's branching conventions with maintainers before creating long-lived branches.

## Integration & dependencies

- Uses `github-pages` gem (see `Gemfile`) which pins a set of supported Jekyll plugins. Use Bundler to ensure parity with GitHub Pages.
- Site is a static site — there are no back-end services or APIs in the repo. External assets (images) are stored under `fig/` or `assets/`.

## Minimal checklist for automated changes

1. Edit source files (not `_site/`).
2. Run `bundle install` then `bundle exec jekyll build` (or `serve`) and verify `_site/` contains expected output.
3. Commit only source files and styles; do not commit generated files under `_site/`.

## Common front-matter fields & examples

Automated edits frequently need to create or update `_posts/` files. These front-matter fields are used across the repo:

- `layout` — typically `meeting` or `workshop` for posts under `_posts/`.
- `title` — page title shown in listings and the page header.
- `date` — ISO date (also used in the filename `YYYY-MM-DD-...`).
- `tags` / `categories` — simple arrays used for filtering and lists.
- `permalink` — optional, when a custom URL is required.
- `location`, `instructors`, `duration`, `description` — used on workshop pages when present.

Example: community meeting post (minimal)

```
---
layout: meeting
title: "Community meeting — Month DD, YYYY"
date: 2025-10-20
tags: [community, meeting]
---
```

Example: workshop post (more fields)

```
---
layout: workshop
title: "Introduction to Shell"
date: 2025-10-07
tags: [shell, workshop]
location: "UCSB Library"
instructors: ["Jane Doe", "John Smith"]
duration: "2 hours"
description: "Introductory shell workshop for reproducible research."
---
```

When adding posts, follow the filename convention: `_posts/YYYY-MM-DD-short-title.md` and keep front-matter keys lowercase and simple arrays for tags.

## PR checklist for automated agents

Use this checklist when an automated agent opens a branch/PR with changes:

1. Target branch: open PRs against `dev-instructors` (confirm with maintainers for long-lived work).
2. Build verification:
	- Run `bundle install` and `bundle exec jekyll build` locally.
	- Inspect `_site/` for the generated page under the expected path.
3. No generated files: ensure `_site/` is unchanged and not included in the commit.
4. Layout & asset checks:
	- Verify `layout:` front-matter matches a file in `_layouts/` (confirm filenames use `.html` extensions; `_layouts/default.html` is the corrected file).
	- If adding images, place them under `fig/` or `assets/` and verify paths in the generated HTML.
5. Style/CSS changes: edit `css/ucsb.css` or `css/simple.css`; re-run the build and confirm styles apply.
6. PR description: include brief summary, files changed, and the generated site path for reviewers to preview.
7. Small automated edits only: prefer one logical change per PR (e.g., one new post or a small CSS tweak).

## Where to look for examples

- See `_posts/` for front-matter examples (meeting/workshop posts).
- See `_layouts/workshop.html` and `_layouts/meeting.html` for page structure.
- See `css/ucsb.css` for existing CSS conventions.

## If you need more context

- Ask the maintainers for the preferred branch strategy and whether the corrected `_layouts/default.html` filename is expected or should be different.

---
If anything here is unclear or you'd like more detail (e.g., common front-matter fields used in `_posts/` or examples of how to add a new page), tell me which area to expand and I'll iterate.
