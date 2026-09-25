# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A hand-written static GitHub Pages site (https://yu-chen-yeh-franky.github.io/) that serves as a personal landing page plus a marketing/support page for each app the author publishes. No build step, no package manager, no tests, no framework. Edit the HTML/CSS directly and push to `main`; GitHub Pages deploys it.

To preview locally, open the `.html` files in a browser or run any static server from the repo root (e.g. `python -m http.server`).

## Site structure

- `index.html` – landing page (avatar, email copy button, links to Apps / YouTube / GitHub / Medium / Printables / Support).
- `show_all_app.html` – grid of app cards; each card links to a `*_Page.html` and shows platform tags (`app-tag`) and a free/paid tag (`app-tag-gray`).
- `<App_Name>_Page.html` – one page per app (currently SkipFrog for YouTube, VibeCraft Timer, Predict Card Magic Trick, Note Card Copy Cat, Princess Card Trick).
- `support.html` – contact info, Google Form link, general FAQ.
- `main.css` – the single shared stylesheet for all pages.
- `assets/app_icon/<App_Name>_icon.png` – app icons, referenced by both the card grid and the app page (also used as `og:image`).
- `sitemap.xml`, `robots.txt`, `google*.html` – SEO / Search Console verification. Don't touch the Google verification files.

## Per-app page pattern

Every `*_Page.html` follows the same template (copy an existing one when adding an app):

1. `<head>` SEO block: `<title>`, `meta description`, `canonical`, Open Graph + Twitter meta, and a `SoftwareApplication` JSON-LD script. All of these must agree with each other (same title/description/image/URL). iOS apps also carry `<meta name="apple-itunes-app" content="app-id=…">`.
2. Font Awesome 6.5.0 from cdnjs, Tailwind v4 browser CDN (`@tailwindcss/browser@4`, used only for small utility classes like `flex justify-center`, `mr-2`, `mb-2`), then `main.css`.
3. Body: Home/Apps nav buttons → app icon → `<h1>` + `.subtitle` → store buttons (`.btn` linking to Chrome Web Store / App Store / Google Play) → `.faq-section` accordion → a tall spacer `<div>` at the bottom.
4. Inline `toggleFaq()` script; the first FAQ item is opened by default via `document.querySelector('.faq-item .faq-answer').classList.add('open')`.

## Adding a new app – checklist

1. Add the icon to `assets/app_icon/`.
2. Create `<App_Name>_Page.html` from an existing page; update every SEO field and store link.
3. Add a card to `show_all_app.html` with the right platform / free-paid tags.
4. Add a `<url>` entry to `sitemap.xml`.
5. Optionally add it to the app list in `support.html`.

## Conventions

- **Whenever you modify any `.html` page, update that page's `<lastmod>` in `sitemap.xml`** to today's date (`YYYY-MM-DD`). If the `<url>` entry has no `<lastmod>` yet, add one. Pages not listed in the sitemap (`google*.html`) are exempt.

- `main.css` is loaded with a cache-busting query string (`main.css?v=N`). When changing CSS, bump the version in the pages you want to pick up the change (pages currently use different values; `index.html` has its own).
- Dark theme only: background `#0f0f0f`, cards `#1a1a1a`, accent `#4da6ff`. Reuse the existing classes (`.btn`, `.app-card`, `.faq-*`) rather than adding new ones; page-specific styles go in an inline `<style>` (as `support.html` does).
- Public-facing text is English. Commit messages are written in Traditional Chinese (see `git log`).
- Contact email shown on the site is `franky.2.0808@gmail.com`; the email copy button relies on `#email-text` + `.copy-btn`.
- `dev.md` holds ad-hoc dev notes (currently just the Font Awesome version).
