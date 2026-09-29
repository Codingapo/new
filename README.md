# TodayInfo

A sleek, responsive, static daily-information homepage built with pure HTML, CSS and JavaScript. No framework, build step, Node.js runtime, or server-side dependency is required.

## Structure

- `index.html` — semantic homepage with SEO metadata, Open Graph tags, JSON-LD, ad placeholders and accessible sections
- `privacy.html` — starter privacy page to customize before launch
- `assets/styles.css` — responsive design system with light/dark mode variables
- `assets/app.js` / `assets/app-v2.js` — API-first router, link-aware content renderer, search, filters and local navigation
- `data/api-config.js` — public ZABursaries API base URL used by the homepage
- `assets/favicon.svg`, `assets/og-card.svg` — self-contained branding assets
- `robots.txt`, `sitemap.xml` — search-engine-ready files

## Run locally

Any static server works. For example:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Deploy to Cloudflare Pages

1. Create a new Pages project in Cloudflare.
2. Connect the repository or upload this folder.
3. Set the build command to empty / none.
4. Set the output directory to `/` (the project root).
5. Deploy.

Because all paths are relative and there is no server runtime, the same folder also works with Cloudflare Workers Static Assets. A minimal Worker configuration can serve the folder using the `assets.directory` binding in `wrangler.toml`.

## Live API integration

The site loads the complete public API surface from the published ZABursaries API configured in `data/api-config.js`: `/site`, `/pages`, `/bursaries`, `/articles`, `/categories`, `/tags`, `/links`, `/search`, and `/crawl/status`. Collection requests follow API pagination automatically. API-provided internal links are rendered as visible, styled links both inline and in a related-links panel; all internal navigation stays inside TodayInfo. If the API is unavailable, the site shows an explicit error state and never substitutes mock content.

The included configuration points to the published API at `https://zabursapi-fqajgker.manus.space/api/v1`. To point the site at another deployment, change `window.TODAYINFO_API_BASE` in `data/api-config.js` to that public `/api/v1` origin. The API must allow CORS for the static site's domain.

## Local route map

- `/` — API-backed home dashboard
- `/latest` — paginated latest normalized records
- `/explore` — pages, categories, topics and internal links from the API
- `/search?q=...` — API-powered search results
- `/categories` — category catalog
- `/tags` — topic catalog
- `/category/:slug` — category/page detail rendered locally
- `/topic/:slug` — tag-filtered records rendered locally
- `/article/:slug` — article detail rendered locally
- `/bursary/:slug` — bursary detail rendered locally
- `/page/:source-path` — any API page, including nested paths such as `/page/government-bursaries-south-africa/department-of-environment-forestry-and-fisheries-bursary`
- `/extract?url=...` — API-normalized source-path extraction rendered locally

Internal API links are translated into these TodayInfo routes, including nested source paths. The API wildcard page route is used for nested records, so links do not degrade to `Cannot GET` or lose their parent path. Source content is fetched as JSON and rendered inside TodayInfo. If an API request fails, TodayInfo displays an explicit API error state; it does not silently substitute mock or bundled editorial content.

## Before production

- Set the production domain in your hosting metadata and sitemap when deploying; route metadata is updated dynamically by the client.
- Replace the privacy-page placeholders with your legal entity, contact details, cookie policy and consent language.
- Add the official Google AdSense script only after approval. The homepage includes clearly marked header, sidebar and footer-compatible ad regions; keep ad dimensions stable to protect Core Web Vitals.
- Connect the newsletter form to a real provider before collecting email addresses.

## AdSense placement guidance

Ad slots are intentionally empty and reserve layout space. Keep them away from primary buttons and do not make ads look like editorial cards. For in-content placements, insert a reserved `.ad-slot` between story groups rather than shifting cards after load.
