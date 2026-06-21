# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static HTML5 portfolio/CV site for Timi Ogunmakin, hosted on GitHub Pages. No build step, no package manager, no test suite.

## Running Locally

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Architecture

Single-page portfolio (`index.html`) plus a separate contact page (`Contact.html`).

- **`css/styles.css`** — custom styles (minified in production); edit this for visual changes
- **`js/scripts.js`** — source JavaScript; `scripts.min.js` is the minified version loaded by the HTML
- **`libs/font-awesome/`** — vendored Font Awesome icon library
- **`_config.yml`** — minimal Jekyll config (only sets `theme: jekyll-theme-slate`)

### Key integrations

- **Formspree** (`f/meqnzdpo`) — handles contact form submissions in `Contact.html`; no server-side code needed
- **jQuery 1.12.4** — loaded from CDN; all custom JS in `scripts.js` assumes jQuery is available
- **Bootstrap** — vendored in `css/bootstrap.min.css`; layout uses Bootstrap grid classes
- **Dark mode** — implemented via `localStorage` + `prefers-color-scheme` media query in `scripts.js`

### Deployment

Pushing to `main` triggers GitHub Pages to deploy automatically. The live site is `timiogunmakin.github.io`. Jekyll processes the repo but the only Jekyll config is the slate theme override — the actual HTML files bypass Jekyll templating.
