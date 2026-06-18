# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of standalone HTML widgets designed to be embedded into [Notion.so](https://www.notion.so/) pages via the `/embed` block. Published via GitHub Pages (Jekyll theme: `jekyll-theme-minimal`). Each widget is a self-contained file with no build step.

## Running Locally

Serve any widget with a static HTTP server from the repo root:

```bash
python3 -m http.server 8000
# Then open http://localhost:8000
```

There is no build, no compile step, no package manager, no test suite, and no linter.

## Architecture

**Every widget is a single self-contained `.html` file.** CSS lives in `<style>` tags and JS lives in `<script>` tags within that same file. There are no shared stylesheets or shared JS modules across widgets — each is fully independent.

External libraries are loaded exclusively via CDN (no local dependencies):
- **D3.js + TopoJSON** — `world-clock/`, `USAirportDistanceMap/`
- **Vue.js** — `USAirportDistanceMap/` (SVG-based interactive map)
- **React** (via Skypack ESM CDN) — `pomodoro/`
- **GSAP** — `USAirportDistanceMap/` animations
- **jQuery** — some converter widgets
- **Font Awesome, Google Fonts** — icons and typography throughout

`index.html` at the root is the gallery/entry point listing all available widgets.

## Key Conventions

- **Self-contained files**: when adding or editing a widget, everything (HTML, CSS, JS) stays in that single file. Do not create shared CSS or JS files.
- **File naming**: kebab-case for HTML files (`time-left-in-2021.html`), camelCase for multi-word directory names (`USAirportDistanceMap/`, `world-clock/`).
- **No transpilation**: plain ES5/ES6 vanilla JS for most widgets. Only the pomodoro uses ESM imports (React via Skypack). Do not introduce TypeScript or a bundler.
- **CSS variables** for theming where applicable (see `button.html`); flexbox/grid for layout; responsive units throughout.
- **Notion embed constraints**: widgets must work inside an iframe with no server-side logic. Keep everything client-side.
- **`index.html`** should be updated when a new widget is added so it appears in the gallery.
