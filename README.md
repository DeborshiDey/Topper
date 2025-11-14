Topper – static website

Overview
- Repository layout:
  - `frontend/public/` — site files served by Netlify (`index.html`, images, SEO files).
  - `frontend/package.json` — optional; no build is required.
  - `netlify.toml` (root) — config that publishes `frontend/public`.

Deploy (Netlify)
- Base directory: `frontend`
- Publish directory: `public`
- Build command: leave blank
- Functions directory: leave blank
- Netlify will serve `frontend/public/index.html` directly.

Custom Domain (Dynadot)
1) In Netlify → Site settings → Domain management → Add your domain.
2) In Dynadot DNS, add:
   - A `@` → `75.2.60.5`
   - A `@` → `99.83.190.102`
   - CNAME `www` → `<your-site>.netlify.app`
3) Wait for propagation and automatic SSL.

Local Development
- Open `frontend/public/index.html` in a browser; no tooling required.
- If you add a build step later (e.g., bundler), update Netlify settings accordingly.

Notes
- Line endings are normalized via `.gitattributes` to avoid CRLF/LF warnings.
- If you prefer a single publish path without a base directory, you can move `public/` to the repo root and set `publish = "public"` in `netlify.toml`.