# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

GitHub Pages **user site** for Joshua Steele, served at https://joshukraine.com (custom domain via `CNAME`). Because this is a user site (`<user>.github.io`), GitHub Pages publishes from the `main` branch root automatically — no Actions workflow, no build step.

This repo hosts the root homepage and standalone subdirectory pages — some permanent (e.g., `/abbie-axel/`, a wedding memorial), some temporary.

## What lives elsewhere (and why it matters)

- **`joshukraine.com/romans-course/`** is a **separate** GitHub Pages project site at [joshukraine/romans-course](https://github.com/joshukraine/romans-course). Both repos publish under the same `joshukraine.github.io` (and therefore `joshukraine.com`) domain — they share the URL space, but the code is **not** in this repo. Never try to edit course content from here.
- **`jsua.co/<slug>`** short URLs that forward here are managed in [joshukraine/jsua-redirects](https://github.com/joshukraine/jsua-redirects) (Cloudflare Pages). When a new page needs a short URL, that work happens in the other repo — edit `redirects.yaml`, regenerate, commit.

## Deployment

- Push to `main` → GitHub Pages serves within ~1 minute. No build, no CI.
- `CNAME` binds the custom domain. Do not delete or rename without coordinating DNS.
- No staging — `main` is production. Preview locally with `bin/dev`, which runs `live-server` on port 8000 with auto-reload (matches the `bin/dev` convention used across Joshua's other projects).

## Pattern: subdirectory-per-page

Add a new page by creating a subdirectory at the root with an `index.html`. The slug is the URL path:

```text
abbie-axel/index.html  →  joshukraine.com/abbie-axel/
```

One page per subdirectory. Use the existing `abbie-axel/index.html` as a starter for one-off info pages.

## Stack for one-off pages

For simple static pages that don't warrant their own repo, use **plain CSS in a single `<style>` block** — no Tailwind, no framework, no build step. `abbie-axel/index.html` shows the pattern:

- Design tokens (fonts, colors) as custom properties on `:root`
- A minimal reset holding only the browser defaults the page would otherwise pick up (body/heading/list margins, list bullets, link color, button and iframe borders) — not a full preflight
- Responsive breakpoints as plain media queries (`640px` small, `768px` medium)
- **Google Fonts** for typography, loaded via `<link rel="stylesheet">` with `preconnect` hints to `fonts.googleapis.com` and `fonts.gstatic.com`
- No JavaScript libraries unless the page has actual interactivity; abbie-axel's lightbox is a native `<dialog>` with a small inline script

Why not the Tailwind CDN: its browser build compiles on every visit (Tailwind's docs call it a prototyping tool, not for production), which slows load and can flash unstyled content. Its `@4` URL is also unpinned, so an upstream release can change the live site with no commit here.

## Images via Cloudinary

Photos for `/abbie-axel/` live in the Cloudinary folder `abbie-axel/`. Cloudinary appends a random suffix to each public ID (`ALV-43_pqscrz`), so take IDs from the Media Library and `curl -I` them — never guess. Uploads over 10 MB / ~25 MP are rejected on this plan; downsize first (4000px long edge, q88) and keep the full-res originals.

Prefer Cloudinary over committing image binaries — keeps the repo lean and gets you responsive format negotiation. Standard transform recipes:

- **In-page display**: `f_auto,q_auto,w_1600,c_limit` — best format per browser, capped width, no upscaling
- **OG / social preview**: `f_auto,q_auto,w_1200,h_630,c_fill,g_auto` — face-aware crop to standard OG dimensions so heads don't get chopped in iMessage / Slack previews

Drop the file extension from the URL so `f_auto` can negotiate WebP/AVIF/JPG per client.

## Lifecycle for temporary pages

When a page is no longer needed:

1. In `jsua-redirects`: repoint the short URL to its successor (e.g., the new permanent page) or remove the entry
2. Here: `git rm -r <slug>/ && git commit && git push`

Clean removal — no deprecation markers, no "page removed" comments, no redirect HTML.

## Conventions

- One subdirectory per page; slug equals URL path
- Keep the root flat: if a static site generator is later introduced (Jekyll, Astro, etc.), the build output must still land at the repo root or the site will 404
- No linter or test suite currently — update this file if that changes
