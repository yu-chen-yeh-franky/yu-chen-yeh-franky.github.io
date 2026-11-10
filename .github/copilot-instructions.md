# Copilot Instructions

## Development and validation

- This is a hand-written static GitHub Pages site. There is no package manager, build command, test suite, or linter.
- Preview a page by opening its HTML file in a browser, or serve the repository root with:

  ```powershell
  python -m http.server
  ```

- For a focused check, open the changed page at `http://localhost:8000/<page>.html`; for the landing page, open `http://localhost:8000/`.
- GitHub Pages deploys the `main` branch. Do not modify the `google*.html` Search Console verification files.

## Site architecture

- `index.html` is the personal landing page and links to the app catalogue (`show_all_app.html`) and support page (`support.html`). It is deliberately minimal: no app cards, app names or FAQ (those live in `show_all_app.html` and the app pages). Its only body text is a one-sentence intro at the bottom; the same sentence is the meta / OG / Twitter description and the `Person` `description` in the site-level JSON-LD (`WebSite`, `ProfilePage`, `Person`) in its `<head>`, so change them together.
- `show_all_app.html` is the app directory. Each card links to a standalone `<App_Name>_Page.html` marketing page and uses the matching image in `assets/app_icon/`. Its `<head>` carries `CollectionPage`, `ItemList` and `BreadcrumbList` JSON-LD; keep the `ItemList` in step with the cards.
- Every app page is self-contained: SEO metadata and JSON-LD are in its `<head>`, and its FAQ markup and `toggleFaq()` behavior are embedded in the page. App pages link to external stores rather than application code in this repository.
- `main.css` supplies shared layout, dark-theme, app-card, button, and FAQ styles. `support.html` keeps its support-page-specific styles inline.
- `sitemap.xml` is the canonical index of publishable pages; `robots.txt` points crawlers to it.

## Page and content conventions

- Public-facing copy is English. Commit messages use Traditional Chinese. Always write the developer / brand name in full as `YU CHEN YEH - Franky` (visible text, meta tags, JSON-LD `author`); the only exception is the `alternateName` lists in `index.html`.
- Retain the dark palette: page background `#0f0f0f`, cards `#1a1a1a`, and accent `#4da6ff`. Prefer the existing shared classes (`.btn`, `.app-card`, `.faq-*`) to new equivalents; put genuinely page-specific styles in that page's inline `<style>`.
- Each app page follows the established structure: Home/Apps navigation, app icon, `h1` and `.subtitle`, store button(s), FAQ accordion, and the bottom spacer. Keep its first FAQ answer opened by default using the existing script convention.
- When changing an app page's title, description, URL, image, app name, or store listing, update every corresponding SEO surface together: document title, description, canonical URL, Open Graph tags, Twitter tags, and `SoftwareApplication` JSON-LD. Keep JSON-LD FAQ and visible FAQ content consistent.
- iOS app pages include `meta[name="apple-itunes-app"]`; preserve or update its app ID when changing the iOS listing.
- Use the full-size app icons in `assets/app_icon/` for social/structured-data images, and the 160×160 `_icon_160.png` copies for every `<img>` (the originals are up to 2 MB). When adding an app, add its page and icons, add its card and `ItemList` JSON-LD entry to `show_all_app.html`, and add its URL to `sitemap.xml`; add it to `support.html` when it should have a support entry.
- Whenever an HTML page listed in `sitemap.xml` changes, update that page's `<lastmod>` to the current `YYYY-MM-DD` date. Add a `<lastmod>` if the entry lacks one.

## Shared stylesheet and external assets

- Pages load `main.css` with a `?v=` cache-busting query. When changing `main.css`, bump the version on every page that should receive the new CSS; preserve the existing per-page versioning rather than assuming one global value.
- Use Font Awesome 6.5.0 from cdnjs for icons. App pages also load the Tailwind v4 browser CDN only for small utility classes already present in the markup.
- The contact email and copy behavior on `index.html` depend on `#email-text` and `.copy-btn`; keep those selectors aligned with `copyEmail()`.
