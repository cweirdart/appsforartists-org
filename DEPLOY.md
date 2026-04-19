# Deploy: appsforartists.org + privatepartyapp.com redirect

One-time setup. Follow in order. Estimated time: 20–40 minutes (most of it is waiting for DNS to propagate).

---

## Part 1 — Push to GitHub

1. Go to <https://github.com/new>.
2. **Repository name:** `appsforartists-org` (or whatever you like — this is just the repo name, not the public site).
3. **Visibility:** Public is simplest. (Private also works with Cloudflare Pages, just one extra "authorize access" click later.)
4. **Do NOT** check "Initialize with README" — we already have one.
5. Click **Create repository**.
6. GitHub shows a "push an existing repository from the command line" block. Copy the remote URL (looks like `https://github.com/YOUR_USERNAME/appsforartists-org.git`).

Back in terminal, in the site folder:

```bash
cd "/path/to/Web: Apps for Artists .org"
git remote add origin https://github.com/YOUR_USERNAME/appsforartists-org.git
git push -u origin main
```

(If you get a credential prompt, sign in via browser or use a personal access token. GitHub Desktop works too if you prefer a GUI.)

Refresh the GitHub repo page — you should see `index.html` and `README.md`.

---

## Part 2 — Connect Cloudflare Pages

1. Go to <https://dash.cloudflare.com> → **Workers & Pages** → **Create** → **Pages** tab → **Connect to Git**.
2. Authorize Cloudflare's GitHub app if asked. Grant access to the new repo (or to all repos).
3. Select `appsforartists-org`. Click **Begin setup**.
4. Build settings:
   - **Project name:** `appsforartists-org` (this becomes your `*.pages.dev` URL)
   - **Production branch:** `main`
   - **Framework preset:** None
   - **Build command:** *(leave blank)*
   - **Build output directory:** `/`
5. Click **Save and Deploy**. Wait ~30 seconds. You'll get a live URL like `https://appsforartists-org.pages.dev` — open it and confirm it looks right.

From now on, every `git push` to `main` auto-deploys.

---

## Part 3 — Move DNS to Cloudflare (for both domains)

Do this once for each of the two domains: `appsforartists.org` and `privatepartyapp.com`.

### 3a. Add the domain to Cloudflare

1. In Cloudflare dashboard → **Add a Site** (top nav, or from the home page).
2. Enter `appsforartists.org`. Click Continue.
3. Choose the **Free** plan. Continue.
4. Cloudflare scans existing DNS records from Namecheap. Review — for this domain there probably aren't many. Keep defaults. Continue.
5. Cloudflare shows two nameservers, e.g.:
   ```
   alice.ns.cloudflare.com
   bob.ns.cloudflare.com
   ```
   Keep this tab open.

### 3b. Update nameservers at Namecheap

1. Log in to Namecheap → **Domain List** → click **Manage** next to `appsforartists.org`.
2. Under **Nameservers**, change the dropdown from `Namecheap BasicDNS` to **Custom DNS**.
3. Paste the two Cloudflare nameservers from the previous step (one per field). Save (green checkmark).
4. Propagation typically takes 5 minutes to a few hours. Cloudflare will email you when it detects the change.

### 3c. Repeat 3a + 3b for privatepartyapp.com

Same exact steps, just with `privatepartyapp.com`.

---

## Part 4 — Attach appsforartists.org to the Pages project

Once Cloudflare confirms `appsforartists.org` is active (Part 3 done for this domain):

1. Cloudflare dashboard → **Workers & Pages** → click your `appsforartists-org` project → **Custom domains** tab → **Set up a custom domain**.
2. Enter `appsforartists.org`. Click Continue → **Activate domain**. Cloudflare auto-adds the needed CNAME record.
3. Repeat for `www.appsforartists.org` (so both the apex and www work). Cloudflare will add the CNAME record for `www` automatically.

HTTPS is automatic — Cloudflare provisions a cert within a couple minutes. Visit <https://appsforartists.org> to confirm.

---

## Part 5 — Set up privatepartyapp.com → privateparty.app redirect

Once Cloudflare confirms `privatepartyapp.com` is active (Part 3 done for this domain):

### 5a. Add a placeholder DNS record

Cloudflare needs at least one proxied DNS record on the domain so its rules engine can intercept traffic.

1. Cloudflare dashboard → select `privatepartyapp.com` → **DNS** → **Records** → **Add record**.
2. Add these two records (both proxied — orange cloud ON):
   ```
   Type  Name                      IPv4 address     Proxy status
   A     @                         192.0.2.1        Proxied
   A     www                       192.0.2.1        Proxied
   ```
   (`192.0.2.1` is a reserved "this-never-routes" IP. We're not actually sending traffic there — the redirect rule will catch it.)

### 5b. Create the redirect rule

1. Same `privatepartyapp.com` zone → **Rules** → **Redirect Rules** → **Create rule**.
2. Name: `Redirect to privateparty.app`
3. **When incoming requests match:** → "All incoming requests" (simplest — it's a redirect-only domain).
4. **Then:** Static redirect → **Type:** 301 (Permanent) → **URL:** `https://privateparty.app${http.request.uri.path}` (you can paste that, including the dollar variable) → **Preserve query string:** ON.
5. **Deploy**.

Test in a new tab: `http://privatepartyapp.com` and `http://www.privatepartyapp.com/test?foo=bar` — both should 301 to `https://privateparty.app/test?foo=bar`.

---

## Verification checklist

- [ ] `https://appsforartists.org` loads the showcase site with padlock
- [ ] `https://www.appsforartists.org` also loads (redirects to or serves the same site)
- [ ] `https://privatepartyapp.com` → redirects to `https://privateparty.app`
- [ ] `https://www.privatepartyapp.com` → redirects to `https://privateparty.app`
- [ ] Path is preserved: `https://privatepartyapp.com/anything` → `https://privateparty.app/anything`

---

## Future edits

```bash
cd "/path/to/Web: Apps for Artists .org"
# edit index.html
git add index.html
git commit -m "update copy"
git push
```

Cloudflare picks it up and redeploys in ~30 seconds. Hard-refresh (Cmd+Shift+R) to bypass browser cache.
