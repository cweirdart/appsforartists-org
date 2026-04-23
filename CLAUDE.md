# appsforartists.org — project context

## What this is

A single-page static showcase for the `appsforartists.org` domain. Built as a supplemental element for the **LACMA Art + Technology Lab 2026** grant application (deadline **2026-04-22**) and intended to live on as a free international resource for artists.

Owner: c.weird (Christopher Weir), info@cweird.com. Domain registered at **Namecheap**.

## Contents

- `index.html` — the entire site (HTML + embedded CSS, no JS)
- `README.md` — project overview + editing notes
- `DEPLOY.md` — step-by-step walkthrough for pushing to GitHub, connecting Cloudflare Pages, moving DNS from Namecheap → Cloudflare, and bundling the `privatepartyapp.com` → `privateparty.app` 301 redirect
- `CLAUDE.md` — this file

## Design decisions (already locked in, don't re-litigate)

- **Static, single file.** No build step, no bundler, no JS. System fonts only — nothing fetched from third-party CDNs (keeps faith with the privacy narrative of the tools showcased).
- **Clean & minimal aesthetic.** Serif display headings (`Iowan Old Style / Palatino / Georgia`), sans body. Off-white / warm background. Respects `prefers-color-scheme: dark`.
- **Two sections:** Apps (Artists Bay, Private Party, Foxglove, InfectID, 3Do, MockDrop) + Portfolios (Shervin Tattoo, MonkeyMan, Sierra).
- **Voice:** echoes the LACMA grant draft — *"Listen. Translate. Share." / "tools that respect rather than extract from artists."* Keep the site itself unbranded; the only nod to the maker is a minimal "Enjoying these free tools?" block linking to cweird.com.

## Deployment target

- **Hosting:** Cloudflare Pages (free tier), git-connected to a new GitHub repo.
- **DNS:** moving from Namecheap BasicDNS to Cloudflare nameservers for both `appsforartists.org` and `privatepartyapp.com`.
- **Bundled task:** `privatepartyapp.com` → `https://privateparty.app` 301 redirect, set up as a Cloudflare Redirect Rule on that zone.

## Constraints

- **Free hosting only** — no paid tiers, no paid databases, no paid CDN. Cloudflare Pages and Cloudflare DNS free tier are the target.
- **Don't cross-contaminate** with other c.weird projects (Private Party, Foxglove, Artists Bay etc. have their own repos and their own rollout plans). This repo is *only* the appsforartists.org showcase.

## Editing the site

Project entries live in `index.html` inside `<li class="project">…</li>` blocks. Each has:
- `<h3>` + optional `<a>` for the link
- `.domain` span with the URL (or "In development" / "Coming soon")
- `.status` chip — `live` (green) or `wip` (amber)
- `.pitch` italic tagline
- `.desc` paragraph

To add a project, copy an existing `<li>` block and edit in place.
