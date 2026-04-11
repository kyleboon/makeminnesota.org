# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

makeminnesota.org is a Jekyll static site serving as a directory of makerspaces, hackerspaces, and maker resources across Minnesota. The site is intended for GitHub Pages hosting.

## Build & Development Commands

```bash
# Install dependencies
bundle install

# Serve locally with live reload
bundle exec jekyll serve

# Build site (output to _site/)
bundle exec jekyll build
```

Ruby version: 3.4.2 (see `.ruby-version`)

## Architecture

- **Jekyll static site** with a single-page layout displaying maker resource cards
- **`_data/makers.yaml`** — The primary content file. All makerspace/resource entries live here as a YAML array. Each entry has fields like `name`, `city`, `description`, `features`, `location`, `website`, `membership`/`access`. Some entries use variant fields (`facilities`, `examples`, `locations`, `cities`) for non-standard resource types (universities, events, programs)
- **`_layouts/base.html`** — Single layout wrapping all pages; loads CSS from front matter and includes head/footer partials
- **`_includes/`** — `head.html` (meta tags, Open Graph, CSS) and `footer.html` (copyright, license)
- **`index.html`** — Iterates over `site.data.makers` to render cards
- **`assets/css/main.css`** — All styling; uses CSS custom properties for theming
- **`reference.md`** — Research/reference material for makerspace data (not rendered on the site)

## Key Conventions

- Content changes are data-driven: add/edit entries in `_data/makers.yaml` rather than modifying HTML
- The site uses no Jekyll plugins or themes — plain HTML/CSS with Liquid templating
- Site is licensed CC BY 4.0
