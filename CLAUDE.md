# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page Jekyll static site (Afrikaans content) about Cornie en Sussie Alant in Port Nolloth, derived from the "One Page Wonder" Jekyll port. There is no package.json, test suite, or linter.

## Commands

- Install: `bundle install` (Gemfile uses `jekyll` and `github-pages`)
- Dev server: `bundle exec jekyll serve` (site at http://localhost:4000)
- Build: `bundle exec jekyll build` (output to `_site/`)

## Deployment

Pushes to `main` deploy via `.github/workflows/azure-static-web-apps-*.yml` (Azure Static Web Apps; builds from `/`, serves `_site`). PRs to `main` get preview deployments.

## Architecture

- `index.html` is the only page. It uses the `default` layout and renders `_includes/header.html`, `features.html`, and `footer.html`. Front matter (`heading`, `subheading`) feeds the header.
- `_layouts/default.html` is wrapped by `_layouts/compress.html` (HTML minification). `_includes/navigation.html` exists but is commented out in the layout.
- Content lives in the `features` collection (`_features/NN-name.md`, declared in `_config.yml`). `_includes/features.html` loops over `site.features` and renders each as a section, alternating image side (`pull-right`/`pull-left` via `cycle`). The filename numeric prefix controls ordering. Each file's front matter supplies `id`, `name`, `heading`, `subheading`, and `image` (path under `images/`); the markdown body is the text.
- Styles: `assets/app.scss` is the Sass entry point (compiled to `/assets/app.css`, referenced in `_includes/head.html`), importing partials from `assets/_sass/` (`_main`, `_layout`, `_header`, `_nav`, and a vendored Bootstrap 3 in `_bootstrap.scss` + `bootstrap/`). Custom styles use Bootstrap `@extend`s (e.g. `.img-responsive`, `.text-muted`). `sass_dir` is set to `/assets/_sass` in `_config.yml`.
- JS: `assets/app.js` is the entry script (loaded at the end of the layout), with jQuery and Bootstrap JS vendored in `assets/_js/`.
