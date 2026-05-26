# joshukraine.com

GitHub Pages user site for Joshua Steele — the home for the root domain `joshukraine.com` and various small subdirectory pages served beneath it.

## What lives here

- **`/`** — placeholder homepage
- **`/abbie-axel/`** — save-the-date for Abbie & Axel (August 22, 2026); temporary, will be removed once their permanent TheKnot.com page is live

## What does *not* live here

`joshukraine.com/romans-course/` is served from a **separate** repo: [joshukraine/romans-course](https://github.com/joshukraine/romans-course). Both repos publish under the same GitHub Pages user domain, so they share the URL space, but the code lives in different places.

Short URLs at `jsua.co/<slug>` that forward here are managed in [joshukraine/jsua-redirects](https://github.com/joshukraine/jsua-redirects) (Cloudflare Pages, not GitHub Pages).

## How to add a temporary page

1. Create a subdirectory at the repo root with an `index.html` (e.g., `event-name/index.html`)
2. Push to `main` — the page is live at `joshukraine.com/event-name/` within ~1 minute
3. If you want a short URL, add an entry to `redirects.yaml` in `jsua-redirects`

`abbie-axel/index.html` is a reasonable starting template for save-the-date / one-off info pages: Tailwind v4 + Google Fonts via CDN, Cloudinary for images, OG and Twitter meta tags ready to fill in.

## How to retire a page

1. In `jsua-redirects`, repoint or remove the short URL
2. `git rm -r <slug>/ && git commit && git push`

## Local preview

```bash
bin/dev
```

Runs `live-server` on port 8000 with auto-reload. Requires `live-server` installed globally (it's in [`~/dotfiles/node/.default-npm-packages`](https://github.com/joshukraine/dotfiles)).

## Deployment

Pushed to `main` → live within ~1 minute. No build step, no CI. `CNAME` binds the custom domain `joshukraine.com` — don't delete or rename it without coordinating DNS.

See [`CLAUDE.md`](CLAUDE.md) for the technical patterns and gotchas worth knowing before editing.
