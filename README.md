# CLASS DECK GENERATOR — the builder that creates client Class Decks

**Live deployment target:** https://classdeckgenerator.vercel.app
**Homepage:** the **builder wizard itself** (`index.html`) — *not* a Class Deck.
**Audience:** HMG staff only. Never hand this repo/zip to a client — hand them the ZIP it produces.

---

## 1. How this deployment is structured (V12.1 fix)

| Path | What it is |
|---|---|
| `/index.html` (and `/generate.html`) | The **Class Deck Builder** — the brand form + live preview + ZIP engine. This is what opens at the site root. |
| `/template/` | The **master Class Deck template** (the full HMG-engine deck: teach studio, whiteboard, webcast, recording studio, CBT, join, admin, parent, community…). The builder fetches these files, stamps the client's brand onto them, and packs the ZIP. You can preview it at `/template/index.html`. |
| `/js/generator.js` | Template Engine v3 (V12.1). Loads every template file from `template/…` (with automatic fallback to the site root, so the same engine still works from a deck's own `/generate.html` page). |
| `/css/`, `/js/`, `/assets/` | Styling and support files for the builder UI. |
| `robots.txt` | `Disallow: /` — this is an internal tool, kept out of search engines. |

### The bug this fixes
Previously the "generator" package was the deck template with the builder buried at
`/generate.html` — so deploying it produced a second HMG ACADEMY CLASS DECK site.
Now the builder **is** the homepage, the deck lives only under `/template/`, and the
engine's template loader resolves `template/<file>` first. Deploying this repo gives
you a builder; deploying the deck repo gives you a deck. No overlap.

## 2. Using the builder

1. Open the site root (or run locally: `python3 -m http.server 8080` → `http://localhost:8080/`).
2. Fill the client's details step by step:
   - **Brand name** & **short name** — stamped into every page, title, manifest and PWA name.
   - **Tagline / motto** — landing page + recording footer.
   - **Contacts & socials** — address, phone (WhatsApp derived), email, website, Facebook, X, Instagram, YouTube, LinkedIn, TikTok.
   - **Colours** — primary / accent / background, with live preview.
   - **Logo** — upload the client's logo (embedded into the deck) or leave blank for a generated monogram.
   - **License model** — `lifetime` or `subscription` (enforced by the free in-browser license engine emitted into the client's `js/license.js`).
   - **Site URL** — written into the client's sitemap, robots, manifest and SEO tags.
3. Review the summary → click **Generate** → a ZIP downloads. Built 100% in the browser (JSZip via free CDN); nothing is uploaded anywhere.

## 3. What the generated ZIP contains

| Folder | Contents |
|---|---|
| `<BRAND>-CLASSDECK/` | The client's complete deployable Class Deck: every page and JS module of the V12 engine, branded config (`js/config.js` — with the founder owner account **blanked**), branded license engine, branded landing page, logo/favicon, PWA manifest + service worker, robots/sitemap, security headers, README + DEPLOYMENT-GUIDE + LICENSE-TERMS. |
| `CLASSDECK-GENERATOR/` | A copy of the builder tool for regeneration. |

## 4. Deploying this builder (free)

1. Push this folder's contents to a GitHub repo (e.g. `classdeckgenerator`), `index.html` at the repo root.
2. Vercel → Add New → Project → import the repo → Framework **Other**, no build command → Deploy.
3. Open the site — you should see the **Class Deck Generator wizard**, not a deck. `/template/index.html` shows the master deck.

## 5. Deploying a generated client deck (what you tell the client)

1. Unzip; use the `<BRAND>-CLASSDECK/` folder only.
2. New GitHub repo → upload the folder's **contents** to the repo root.
3. Vercel free import (Framework: Other, no build) — or Netlify Drop / Cloudflare Pages / GitHub Pages.
4. Verify: branded landing page → teacher signup + 3-day trial works → `join.html?room=CODE` joins free → recording downloads and seeks → PWA installs.
5. Issue the client's HMG ACCESS KEY per the chosen license model.

The generated ZIP contains its own `DEPLOYMENT-GUIDE.md` written for the client.

## 6. Customising

- **Template**: edit files under `/template/` — every future generated deck inherits the change.
- **Replacement rules**: `CDGenerator._brand()` in `js/generator.js`.
- **Files packed per client**: the `deckFiles` list in `js/generator.js`.
- **License engine emitted per client**: `CDGenerator._licenseJS()`.
- **Client landing page**: `CDGenerator._landingPage()`.

Free stack, always: static hosting, in-browser generation, no server, no database, **no paid AI API**.
