# Claude Code handoff

Paste the prompt below into Claude Code after `cd`-ing into this folder. It does the parts that need terminal/`gh` access: create the GitHub repo, push, and print a handoff back to you for the Cloudflare + DNS dashboard steps.

---

## One-time: open Claude Code in this folder

```bash
cd "/path/to/Web: Apps for Artists .org"
claude
```

(Or open this folder in your editor if Claude Code is integrated there.)

---

## Paste this prompt into Claude Code

> I have a single-page static site for **appsforartists.org** ready to deploy. The working tree is already a git repo with one or two commits on `main`. Please:
>
> 1. Verify the working tree is clean and on `main` with commits. If not, fix it.
> 2. Check that `gh` is authenticated (`gh auth status`). If not, tell me to run `gh auth login` and stop.
> 3. Create a **public** GitHub repo named `appsforartists-org` under my account (don't initialize with a README — we already have one):
>    ```
>    gh repo create appsforartists-org --public --source=. --remote=origin --push
>    ```
> 4. After the push succeeds, print:
>    - the GitHub repo URL
>    - a reminder that the remaining steps (Cloudflare Pages connect, Namecheap → Cloudflare nameserver change, privatepartyapp.com redirect) are in `DEPLOY.md` parts 2–5 and require dashboard access I'll handle in my browser
>
> Do **not** try to run `wrangler` or log into Cloudflare — those steps need dashboard interaction and I'll do them manually.
>
> **Context** (also in `CLAUDE.md`):
> - Site is for a LACMA Art+Tech Lab 2026 grant supplement, deadline 2026-04-22 — time-sensitive
> - Target hosting: Cloudflare Pages free tier, git-connected (no build step, output `/`)
> - Domain is at Namecheap; nameservers will move to Cloudflare
> - Bundled task: redirect `privatepartyapp.com` → `https://privateparty.app` via a Cloudflare Redirect Rule once that zone is in Cloudflare
>
> Do the steps. Don't ask me to confirm each one unless `gh` isn't authed.

---

## What you do after Claude Code is done

Follow **Parts 2 through 5** of `DEPLOY.md`:

- **Part 2** — Cloudflare Pages: connect to the new GitHub repo, deploy.
- **Part 3** — Add both `appsforartists.org` and `privatepartyapp.com` to Cloudflare, change nameservers at Namecheap.
- **Part 4** — Attach `appsforartists.org` as a custom domain on the Pages project.
- **Part 5** — Create the `privatepartyapp.com` → `privateparty.app` Redirect Rule.

---

## If you'd rather not use Claude Code right now

You can do Part 1 in a plain terminal. Just substitute your GitHub username:

```bash
cd "/path/to/Web: Apps for Artists .org"
# create empty repo at https://github.com/new first, then:
git remote add origin https://github.com/YOUR_USERNAME/appsforartists-org.git
git push -u origin main
```
