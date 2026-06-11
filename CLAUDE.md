# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Jianchuan Chen's personal academic homepage, hosted on GitHub Pages. Built with Jekyll (static site generator), showcasing research in 3D digital human reconstruction, neural rendering (NeRF), and 3D Gaussian Splatting.

## Development Commands

```bash
# Start local development server (watches for changes, serves on localhost:4000)
bundle exec jekyll serve
# or use the provided script:
bash run_server.sh
```

No separate build, test, or lint commands exist — Jekyll handles everything.

## Architecture

**Content flow:** Markdown (`_pages/`) + YAML data (`_data/`) → Liquid templates (`_layouts/`, `_includes/`) → static HTML/CSS/JS in `_site/` (build output, not committed).

**Key files:**
- `_config.yml` — site-wide configuration (author info, URL, plugins, timezone)
- `_pages/about.md` — the homepage (`permalink: /`), main content file
- `_data/navigation.yml` — navigation menu items
- `_layouts/default.html` — root HTML template
- `assets/css/main.scss` — CSS entry point (imports all `_sass/` partials)

**Styling:** SASS partials in `_sass/` are imported by `assets/css/main.scss`. The Jekyll build compiles this to compressed CSS.

**Project subpages** (e.g., `GM-NeRF/`) are self-contained HTML sites nested in the repo root and served as static directories — they are not processed by Jekyll.

**Python utility:** `google_scholar_crawler/main.py` uses the `scholarly` library to fetch citation counts from Google Scholar (reads `GOOGLE_SCHOLAR_ID` from environment, outputs to `results/`). This runs independently of the Jekyll build.

## Gem Dependencies

Dependencies are managed via Bundler. The `Gemfile` uses `gems.ruby-china.com` as the gem source (Chinese mirror). Primary dependency is `github-pages` which pins Jekyll to 3.9.3.
