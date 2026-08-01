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

1. **Create the repo.** On github.com, create a new **public** repository
   (Pages is free on public repos on any plan; private repos need GitHub Pro/paid
   org). Name it whatever you like, e.g. `cislf-website` — it does not need to
   match the domain.
2. **Push this code:**
   ```bash
   cd ~/cislf-website
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```
3. **Enable Pages.** Repo → Settings → Pages → Build and deployment → Source:
   "Deploy from a branch" → Branch: `main`, folder `/ (root)` → Save.
   (GitHub auto-detects Jekyll and builds it for you — no Actions workflow needed.)
4. **Point the custom domain at GitHub.** Still on Settings → Pages, enter
   `cislf.org` under "Custom domain" and save (this writes the `CNAME` file
   for you too, but it's already committed here).

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
- Add a real `assets/images/favicon.png` (32×32 or 64×64) — the layout already
  references it; nothing is there yet since I didn't have your logo file.
- Content, donate link (Zeffy), and the contact form (embedded Google Form)
  were carried over as-is from the current site — see `_config.yml` for the
  URLs if either ever changes.
