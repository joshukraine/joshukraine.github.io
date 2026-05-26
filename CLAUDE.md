# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

GitHub Pages **user site** for Joshua Steele, served at https://joshukraine.com (custom domain via `CNAME`). Because this is a user site (`<user>.github.io`), GitHub Pages publishes from the `main` branch root automatically — no Actions workflow, no build step.

This repo hosts the root homepage and one-off / temporary subdirectory pages (e.g., `/abbie-axel/`) — each a standalone static page.

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

For simple static pages that don't warrant their own repo:

- **Tailwind v4** via CDN: `<script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>`
- Custom theme tokens go in `<style type="text/tailwindcss">` inside an `@theme { ... }` block
- **Google Fonts** for typography, loaded via `<link rel="stylesheet">` with `preconnect` hints to `fonts.googleapis.com` and `fonts.gstatic.com`
- Skip Alpine.js and Tailwind Plus Elements unless the page has actual interactivity — they're dead weight on a static info page

### Tailwind v4 gotcha worth remembering

`class="font-[var(--font-display)]"` does **not** generate a utility in Tailwind v4 — the arbitrary-value syntax with a CSS variable silently fails (no error, just falls back to the inherited font). The correct path:

1. Declare the token inside `@theme`: `--font-display: "Cormorant Garamond", ui-serif, ...;`
2. Use the auto-generated utility: `class="font-display"`

Same idea for `--color-*` (→ `text-*`/`bg-*`) and other Tailwind theme namespaces. When you add a token under `@theme`, the matching utility is generated for free — don't reach for `[var(--…)]` arbitrary syntax.

## Images via Cloudinary

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
