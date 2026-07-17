# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, no-build personal website (recreated from bluefuture.neocities.org) deployed via GitHub Pages from the `main` branch root. There is no package.json, build step, or test suite — every page is plain HTML/CSS/JS served as-is.

## Working with the code

- To preview locally, serve the folder over HTTP rather than opening files directly — YouTube iframe embeds return error 153 ("embedding not allowed") when loaded from a `file://` origin because there's no valid referrer. Run `python -m http.server` from the repo root and visit `http://localhost:8000/<page>.html`.
- Deploy is just `git push` to `main`; GitHub Pages rebuilds automatically (check progress under the repo's Actions tab, "pages build and deployment").
- Styling uses the Tailwind CDN build (`<script src="https://cdn.tailwindcss.com">`, no config file, no `npm install`). Utility classes are used inline; where a reusable/complex rule needs `@apply`, it lives inside a `<style type="text/tailwindcss">` block in the `<head>` of that page rather than in an external stylesheet, since the CDN engine only scans `<style type="text/tailwindcss">` blocks and `class` attributes in the document — it does not process linked `.css` files. `about.css`, `style.css`, and `yuri.css` are plain hand-written CSS (with manual `@media` breakpoints for responsiveness), not Tailwind.

## Page structure

- **index.html** — entry point. A "click to initialize" overlay triggers a typewriter boot sequence (`typeEffect()`), prompts for a name via a plain `<input>` (not a form), echoes it back, then reveals a link into `about.html`. Also runs a `setInterval` that scrambles `document.title` as a cosmetic effect. Fully self-contained (Tailwind CDN + inline `<script>`), no external stylesheet.
- **about.html** — the main site shell: a 3-column CSS grid (`#sidebar` / `#content` / `#rightbar`) that collapses to a single stacked column below the `md` breakpoint. `#content` holds a 4-tab widget (`openTab()` in the trailing `<script>` toggles `.tab-content`/`.tab-button` `active` classes) — main dashboard, changelog, an "about" tab (styled by `about.css`, uses the `.top-grid`/`.cyber-box`/`.stamp-marquee` classes), and a guestbook tab gated behind a client-side password overlay (`checkPassword()` — the check is a plaintext string compare in JS, cosmetic only, not real access control). Depends on `about.css` and `yuri.css`.
- **fav.html** — favourite-characters page, sidebar/content/rightbar layout via Tailwind utility classes directly (no external stylesheet); links out to fandom wiki pages and embeds YouTube iframes.
- **about.css** — about.html-only rules, scoped under `#about-page`/`#tab3` (top-grid, cyber-box widgets, stamp marquee animation, gallery grid, custom scrollbar styling for that tab).
- **yuri.css** — about.html's tab3 "widget section" (gifypet/flag counter/song/poll/status/guestbook windows): `.window`/`.window-title`/`.window-content` card components, `.desktop-section`/`.bottom-row` responsive grids, global scrollbar theming.
- **style.css** — legacy stylesheet, currently unused by any page in the repo (superseded by inline Tailwind utilities); check before deleting in case a page still references it.

## Conventions carried over from the original site

- Fixed-pixel widths from the original Neocities source (e.g. 220px sidebars, 430px overlay windows, 400px iframes) have been converted to `w-full max-w-[Npx]` patterns so they degrade to full-width on mobile instead of causing horizontal overflow — follow this pattern for any newly copied fixed-width block rather than reintroducing a bare `width:Npx`.
- Most images/audio/iframes point at external hosts (file.garden, catbox.moe, YouTube). A few character images (`favcharacters/*.jpg`) and blinkies (`website-stamps/*.gif`) are referenced but not present in the repo — they were part of the original Neocities upload and were never migrated.
