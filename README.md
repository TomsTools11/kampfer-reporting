# kampfer-reporting

Static hosting for **The Kampfer Agency — Campaign Configuration Overview**.

The page is a single self-contained HTML file: all styles, the one image (an
inline `data:` URI), and the one script are embedded. The only external request
is to Google Fonts for the Inter typeface. There is no build step and no
dependencies.

## Layout

| Path          | Purpose                                                    |
| ------------- | ---------------------------------------------------------- |
| `index.html`  | The report — served at the deployment root (`/`)            |
| `vercel.json` | Vercel project configuration (framework, headers, URLs)     |
| `robots.txt`  | Blocks search-engine indexing (see note below)              |

## Deploying to Vercel

1. In Vercel, choose **Add New… → Project** and import `TomsTools11/kampfer-reporting`.
2. Leave every build setting at its default:
   - **Framework Preset:** Other (set by `"framework": null` in `vercel.json`)
   - **Build Command:** none
   - **Output Directory:** none — the repository root is served as-is
   - **Install Command:** none
   - **Root Directory:** `./`
3. Click **Deploy**. The report is served at the deployment root.

No environment variables and no secrets are required.

Pushes to `main` publish to production; pushes to any other branch produce a
preview deployment.

## Configuration notes

- `cleanUrls` is on, so `/index.html` also resolves at `/`.
- Security headers (`X-Content-Type-Options`, `X-Frame-Options`,
  `Referrer-Policy`) are applied to all routes.
- HTML is served with `Cache-Control: public, max-age=0, must-revalidate` so an
  updated report reaches viewers immediately rather than being served from a
  stale browser cache.
- `robots.txt` currently disallows all crawlers, on the assumption that a
  client's campaign configuration should not turn up in search results. Delete
  the file if you want the page indexed. Note that this does not make the page
  private — anyone with the URL can open it. For real access control, enable
  Vercel's Deployment Protection (password or SSO) on the project.

## Updating the report

Replace `index.html` with the new export and push. To host more than one report,
add further `.html` files at the repository root; each is served at its own path
(for example `q3-review.html` → `/q3-review`).
