# cislf.org — GitHub Pages site

Source for the Central Indiana Scouting Legacy Foundation website, rebuilt as a
small static [Jekyll](https://jekyllrb.com/) site for GitHub Pages. Given the
site's traffic (four pages, low volume), this deliberately has **no build
tooling, no JS framework, and no backend** — just Jekyll's built-in templating
(free on GitHub Pages) so the nav/footer aren't duplicated four times.

## Local preview

```bash
bundle install
bundle exec jekyll serve
# open http://localhost:4000
```

## Structure

- `_config.yml` — site title, org details, donate/contact-form URLs (edit these,
  not the page files, when a link changes)
- `_layouts/default.html`, `_includes/nav.html`, `_includes/footer.html` — shared chrome
- `index.md`, `our-units.md`, `contact-us.md`, `donate.md` — the four pages
- `assets/css/style.css` — all styling, no external CSS framework
- `CNAME` — pins the custom domain (`cislf.org`) for GitHub Pages
- `404.html` — custom not-found page

## One-time setup: publish to GitHub Pages

This repo is [dogara78-byte/CISLF](https://github.com/dogara78-byte/CISLF) —
**public**, so Pages is free (private repos need GitHub Pro/paid org).

1. **Push this code** (already done — repo is public, `main` is the default branch):
   ```bash
   cd ~/cislf-website
   git push -u origin main
   ```
2. **Enable Pages.** Repo → Settings → Pages → Build and deployment → Source:
   "Deploy from a branch" → Branch: `main`, folder `/ (root)` → Save.
   (GitHub auto-detects Jekyll and builds it for you — no Actions workflow needed.)
3. **Confirm the custom domain.** Still on Settings → Pages, `cislf.org` should
   already show under "Custom domain" (the repo already had a `CNAME` file with
   that value before this code was pushed).

## DNS changes (at your domain registrar, not GitHub)

Since `cislf.org` is currently just forwarding to the Google Sites URL, you'll
replace that forwarding with real DNS records pointing at GitHub:

**Apex domain (`cislf.org`)** — four `A` records pointing at GitHub's Pages IPs:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**`www.cislf.org`** (recommended, so both work) — one `CNAME` record:
```
www.cislf.org.  CNAME  <your-username>.github.io.
```

Remove/replace whatever URL-forwarding or existing A/CNAME records your
registrar currently uses for the Google Sites redirect.

DNS propagation can take up to ~24–48 hours (usually much faster). Once it
resolves, go back to Settings → Pages and check **Enforce HTTPS** — GitHub
issues a free TLS certificate automatically once DNS is verified.

## After go-live

- Retire the Google Site (Settings → General → Delete site) once you've
  confirmed `cislf.org` is serving the new site correctly — no need to keep
  paying attention to two copies.
- Content, donate link (Zeffy), and the contact form (embedded Google Form)
  were carried over as-is from the current site — see `_config.yml` for the
  URLs if either ever changes.

## Images

Real logo, badge, and photo assets live in `assets/images/` (web-optimized:
resized and converted to WebP/compressed JPEG from the originals). The
full-resolution originals are kept locally in `assets/images/incoming/` as an
archive but are gitignored — not pushed to GitHub, since a static site has no
use for multi-megabyte source files. If you add new photos later, drop
originals in `incoming/` and resize/compress before adding them to
`assets/images/` proper (keeps the site fast to load).
