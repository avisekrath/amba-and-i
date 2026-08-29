# Amba & I

A daily fitness tracker for Avisek and Amba — training, walks, meals, prep, all in one page.

## What's here

Static PWA, one HTML file plus a service worker. No build step, no framework. State is stored in `localStorage` per user (`a` / `i`) and per date, so both partners can share one install and each toggle their own view.

- `index.html` — the app
- `manifest.webmanifest` — install metadata
- `sw.js` — offline cache (cache-first, background revalidate)
- `icon.svg`, `icon-192.png`, `icon-512.png`, `icon-180.png`, `favicon.png`, `icon-maskable-512.png` — app icons
- `_headers` — Cloudflare Pages cache rules

## Run it locally

Any static server works. From this directory:

```
python3 -m http.server 8080
```

Then open `http://localhost:8080`. Service workers need a real origin — `file://` won't register the SW.

## Deploy on Cloudflare Pages

1. Push this repo to GitHub.
2. In the Cloudflare dashboard: **Workers & Pages → Create → Pages → Connect to Git**.
3. Pick this repo. Framework preset: **None**. Build command: leave blank. Output directory: `/` (root).
4. Deploy. Cloudflare gives you an `*.pages.dev` URL.
5. Add a custom domain if you want (e.g. `daily.avisekrath.com`) in the Pages project settings.

The `_headers` file makes sure `sw.js` isn't cached at the edge, so future updates ship on the next visit.

## Deploy on GitHub Pages (alternative)

1. Push to GitHub.
2. **Settings → Pages → Source: Deploy from a branch, branch: `main`, folder: `/ (root)`**.
3. Site publishes at `https://<user>.github.io/<repo>/`. (`_headers` is ignored by GH Pages — SW updates may take one extra reload.)

## Install on your phone

**iPhone (Safari):** Share menu → **Add to Home Screen**. Icon appears; it launches full-screen.

**Android (Chrome):** The in-app "Install" banner appears once the SW registers, or use the browser menu → **Install app**.

## Updating

Edit anything, push to `main`, Cloudflare/GH auto-deploys. When you change `index.html` or the assets in `SHELL` in `sw.js`, bump `CACHE_VERSION` in `sw.js` so old caches get replaced on the next visit.

## Data & privacy

Everything stays in the browser — no backend, no analytics, no third-party requests. If you clear site data, you lose the check history.
