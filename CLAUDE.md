# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

GitHub Pages **user site** for Joshua Steele, served at https://joshukraine.com (custom domain via `CNAME`). Because this is a user site (`<user>.github.io`), GitHub Pages publishes from the `main` branch root automatically — there is no Actions workflow or build step.

## Current State

The repo is a placeholder: a single static `index.html` ("Coming soon.") and a `CNAME` file. There is no framework, no package manager, no test suite, and no linter configured. When introducing any of those, update this file with the relevant commands.

## Deployment

- Push to `main` → GitHub Pages rebuilds and serves the site within ~1 minute.
- DNS for `joshukraine.com` must point at GitHub Pages IPs; the `CNAME` file binds the custom domain to this repo. Do not delete or rename `CNAME` without coordinating a DNS change.
- No staging environment — `main` is production. Preview locally before pushing (e.g., `python3 -m http.server` from the repo root, then open `http://localhost:8000`).

## Conventions

- Keep the root layout flat enough that GitHub Pages can serve files directly. If a static site generator is later introduced (Jekyll, Astro, etc.), the build output must still land at the repo root or the site will 404.
