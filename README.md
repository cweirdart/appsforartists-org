# appsforartists.org

Single-page static showcase. No build step, no dependencies.

## Files

- `index.html` — the whole site (HTML + embedded CSS)
- `README.md` — this file

## Local preview

Just open `index.html` in a browser. Or serve it:

```
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy (all free)

### Option 1 — Cloudflare Pages (recommended)

1. Push this folder to a new GitHub repo (e.g., `appsforartists-org`).
2. Go to Cloudflare → Workers & Pages → Create → Pages → Connect to Git.
3. Select the repo. Build command: *(leave blank)*. Output directory: `/`.
4. Deploy. You'll get a `*.pages.dev` URL immediately.
5. In Cloudflare → Pages → your project → Custom domains, add `appsforartists.org`.
6. Point the domain's DNS to Cloudflare (or if it's already on Cloudflare, just hit "activate"). Cloudflare handles HTTPS automatically.

### Option 2 — GitHub Pages

1. Push to a GitHub repo.
2. Settings → Pages → Source: `main` branch, `/` root. Save.
3. Add CNAME file: `echo "appsforartists.org" > CNAME` and commit.
4. At your registrar, point `appsforartists.org` at GitHub Pages:
   - A records: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - Or CNAME `www` → `<username>.github.io`
5. In Settings → Pages, set the custom domain and enable HTTPS.

### Option 3 — Vercel

1. `vercel` in this folder (install CLI if needed), or import the GitHub repo at vercel.com.
2. Framework preset: *Other* (it's a static site).
3. Add custom domain `appsforartists.org` in project settings and update DNS per Vercel's instructions.

## Bonus: redirect privatepartyapp.com → privateparty.app

While you're in your DNS registrar (or Cloudflare), set up a 301 redirect for `privatepartyapp.com`:

- **Cloudflare:** add the domain to Cloudflare, then Rules → Redirect Rules → "Redirect from URL" → match all paths on `privatepartyapp.com` → `https://privateparty.app/$1` (301).
- **Registrar-level:** most registrars (Namecheap, Porkbun, etc.) have a built-in URL forwarding feature in the domain settings — set it to `https://privateparty.app` with permanent (301) redirect.

## Editing

- All project copy lives in `index.html` inside `<li class="project">` blocks.
- Status labels are `<span class="status live">Live</span>` or `<span class="status wip">In progress</span>`.
- Pitch lines (italic quotes) are `<p class="pitch">…</p>`.
- To add a project, copy an existing `<li class="project">…</li>` block and edit in place.

## Design notes

- Clean/minimal, system fonts only (no Google Fonts fetch — nothing leaks to third parties, faithful to the privacy framing of the tools showcased).
- Respects `prefers-color-scheme: dark`.
- Max content width ~720px, responsive down to phones.
- No JavaScript. No analytics. No trackers.
