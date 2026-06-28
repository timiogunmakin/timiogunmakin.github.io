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

Two HTML pages: `index.html` (portfolio/CV) and `photos.html` (photography gallery). Both share the same header, footer, and CSS/JS files.

### Files

- **`css/styles.css`** — all custom styles; CSS custom properties (tokens) at the top define colors, fonts, and spacing. Dark mode is implemented by swapping token values under `[data-theme="dark"]`.
- **`js/scripts.js`** — jQuery-dependent shared behavior: nav scroll animation, mobile menu open/close, `#experience-timeline` builder. Loaded by both pages; `scripts.min.js` exists but is not loaded anywhere.
- **`images/`** — static assets served directly; add photos here to use them in `photos.html`.
- **`libs/font-awesome/`** — vendored Font Awesome icons.
- **`css/bootstrap.min.css`** — vendored but not loaded; leftover from a previous design.

### Key integrations

- **Formspree** (`f/meqnzdpo`) — handles contact form submissions in the `#contact` section of `index.html`; no server-side code needed.
- **jQuery 1.12.4** — loaded from CDN; `scripts.js` assumes jQuery is available.
- **Google Fonts** — Inter, JetBrains Mono, Space Grotesk; loaded from CDN in both page `<head>`s.

### Theme / dark mode

Theme init, sticky-header scroll handler, and the theme toggle button listener are **inlined at the bottom of each HTML file** (not in `scripts.js`). `data-theme` is set on `<html>` at page load from `localStorage`; the default is `auto` (follows `prefers-color-scheme`).

### Photography page (`photos.html`)

Uses CSS-only masonry (`columns: 3 260px`). The lightbox (also inlined in `photos.html`) activates on any `<figure class="photo-item">` that has a `data-src` attribute. Placeholder tiles (`<div class="photo-ph r-*">`) have no `data-src` so they are not clickable.

Photos are hosted on **Cloudinary** (cloud name: `dutkmvhub`). To add a photo:
1. Upload the image in the Cloudinary dashboard (Media Library → Upload).
2. In `photos.html`, replace `<div class="photo-ph r-*"></div>` with:
   ```html
   <img src="https://res.cloudinary.com/dutkmvhub/image/upload/q_auto,f_auto/PUBLIC_ID"
        alt="Description" loading="lazy">
   ```
3. Add `data-src="https://res.cloudinary.com/dutkmvhub/image/upload/q_auto,f_auto/PUBLIC_ID"` and `data-caption="Title"` to the parent `<figure>` tag.

The `q_auto,f_auto` transform parameters auto-optimize quality and serve WebP/AVIF — always include them.

### Deployment

`.nojekyll` is present, so GitHub Pages skips Jekyll processing entirely. `_config.yml` has no effect. Pushing to `main` deploys to `timiogunmakin.github.io` automatically.
