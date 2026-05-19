# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Matt DeButts's personal academic website, served at **mattdebutts.com** (see `CNAME`). It is a Jekyll site built with the **Minimal Mistakes** theme as a `remote_theme`, deployed by GitHub Pages from the `master` branch. There is no build step in this repo — pushing to `master` triggers a GitHub Pages build.

## Local development

```bash
bundle install                # first time only
bundle exec jekyll serve      # serve at http://localhost:4000 with live reload
bundle exec jekyll build      # one-off build into _site/
```

`_config.yml` is **not** reloaded by `jekyll serve` — restart the server after editing it.

## Architecture

The site is content-driven via Jekyll conventions. There is no application code; understanding the structure is mostly about knowing which file controls which thing.

- **`_config.yml`** — site-wide settings, including theme skin, plugins, analytics (`google-gtag` with the GA4 tracking ID), the `author:` sidebar links (CV, Scholar, ORCID, etc.), and the `footer:` links. `robots: "noindex,nofollow"` is currently set — toggle this when going public. Page front matter inherits the defaults defined under `defaults:` (layout: single, author_profile: true).
- **`_data/navigation.yml`** — the top nav bar (About / Research / Teaching / Journalism / CV). The CV link points to `/assets/DeButts_CV.pdf`.
- **`_pages/`** — the actual site content. Each page sets its own `permalink:` in front matter; that is what determines the URL, not the filename. `about.md` is the homepage (`permalink: /`). `category-archive.md`, `tag-archive.md`, and `year-archive.md` are template archive pages from the theme starter.
- **`_posts/`** — sample posts from the Minimal Mistakes starter template; not currently surfaced in the nav. Safe to leave alone unless adding a blog.
- **`_includes/head/custom.html`** — injected into `<head>`. Sets the favicon and contains a small JS snippet that, on mobile, **replaces the "About" nav link text with the name of the current page** (Research / Teaching / Journalism). This is paired with the `.nav-mobile-text` / `.nav-desktop-text` CSS classes in `main.scss`.
- **`assets/css/main.scss`** — all custom styling, layered on top of the Minimal Mistakes theme. Front matter dashes at the top are required so Jekyll processes it. Notable customizations:
  - Font stack and `$type-size-*` / `$max-width` overrides are tuned to match a reference site ("Yingdan's") — keep these consistent if changing typography.
  - Square (non-circular) author avatar capped at 200px (`.author__avatar img`).
  - Sidebar fade-on-hover from the theme is disabled (`.sidebar { opacity: 1 !important }`).
  - The large `.page__title` block on each tab is hidden by design.
  - Mobile (`max-width: 64em`) hides the sidebar entirely and shows `.mobile-profile` inline on the About page instead.
  - `.journalism-item` / `.journalism-logo` / `.journalism-text` style the publication rows on `_pages/journalism.md`, which uses inline HTML divs rather than a Liquid loop.
- **`assets/images/logos/`** — publication logos referenced from `journalism.md`.
- **`Yingdan_Latex_CV/`** — unrelated working files for the user's CV; not part of the site build.

## Editing patterns to follow

- **Site-wide config** (analytics, theme skin, social links, nav) belongs in `_config.yml` or `_data/navigation.yml`, not in HTML includes. Memory note: prefer `_config.yml`'s `analytics.provider: google-gtag` over a hand-written `<script>` tag.
- **Page-specific tweaks** go in the page's front matter (e.g. `classes: wide`, `author_profile: true`).
- **Styling changes** go in `assets/css/main.scss`. The theme is a remote theme, so you cannot edit theme source files directly — override via SCSS variables (before the `@import "minimal-mistakes"` line) or by adding rules after the import.
- The CV PDF is hardcoded at `/assets/DeButts_CV.pdf` in both `_config.yml` and `_data/navigation.yml` — replacing the CV means overwriting that file (don't rename it) or updating both references.
