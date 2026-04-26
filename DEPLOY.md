# Deploying Orochiverse

`index.html` is a single self-contained file. No build step, no bundler, no
runtime — just static hosting. Pick whichever target below fits.

## What gets deployed

```
index.html      ← the only required file
```

External dependencies (loaded by the browser, no action needed from you):

- Google Fonts CSS (`fonts.googleapis.com` + `fonts.gstatic.com`) — `Space Grotesk` + `JetBrains Mono`.

Everything else (CSS, hero canvas, scroll/reveal/form JS, SVG marks) is inlined.

## Optional polish before you ship

- **Favicon.** `exports/logo/favicon.png` exists in the design bundle — copy it
  next to `index.html` and add `<link rel="icon" href="/favicon.png">` to
  `<head>`.
- **OpenGraph image.** Set `og:image` to a hosted preview (1200×630). The
  `og:type`, `og:title`, `og:description`, `og:site_name` tags are already
  wired.
- **Canonical URL.** Once you know the production hostname, add
  `<link rel="canonical" href="https://orochiverse.com/">`.
- **Form backend.** The contact form opens the visitor's mail client via
  `mailto:`. If you'd rather collect submissions server-side, point the form
  at Formspree / Basin / Netlify Forms / your own endpoint and replace the
  `submit` handler in the second `<script>` block.

## Option A — Netlify

```bash
# from the repo root
npx netlify deploy --dir=. --prod
```

Or drag-and-drop the folder onto <https://app.netlify.com/drop>.

`netlify.toml` (optional):
```toml
[build]
  publish = "."

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    Referrer-Policy = "strict-origin-when-cross-origin"
    Permissions-Policy = "geolocation=(), microphone=(), camera=()"
    Cache-Control = "public, max-age=300, must-revalidate"
```

## Option B — Vercel

```bash
npx vercel --prod
```

When prompted, accept the defaults. Vercel auto-detects a static site.

## Option C — Cloudflare Pages

1. Push the repo to GitHub/GitLab.
2. Cloudflare dashboard → **Workers & Pages** → **Create** → **Connect to Git**.
3. Build command: *(leave blank)*. Output directory: `/`.
4. Save and deploy.

## Option D — GitHub Pages

```bash
git init && git add index.html DEPLOY.md && git commit -m "Initial site"
git branch -M main
git remote add origin git@github.com:<you>/<repo>.git
git push -u origin main
```

Then in repo settings → **Pages**:
- Source: **Deploy from a branch**
- Branch: `main` / `/ (root)`

Site goes live at `https://<you>.github.io/<repo>/` within a minute.

> ⚠️ The site uses absolute paths (`#top`, `#contact`). On a project page
> served from a sub-path, in-page anchor links still work — but if you add a
> favicon, reference it as `./favicon.png`, not `/favicon.png`.

## Option E — S3 + CloudFront

```bash
aws s3 cp index.html s3://orochiverse-site/index.html \
  --content-type "text/html; charset=utf-8" \
  --cache-control "public, max-age=300, must-revalidate"

aws cloudfront create-invalidation \
  --distribution-id <DIST_ID> --paths "/index.html" "/"
```

Bucket needs:
- Static website hosting enabled, **Index document** = `index.html`.
- A bucket policy that allows public read **only via the CloudFront OAC**, not
  the bucket URL.
- CloudFront with HTTPS enforced (ACM cert in `us-east-1`).

## Option F — Plain VPS (nginx)

```nginx
server {
  listen 443 ssl http2;
  server_name orochiverse.com www.orochiverse.com;

  root /var/www/orochiverse;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
    add_header Cache-Control "public, max-age=300, must-revalidate";
  }

  # security headers
  add_header X-Frame-Options "DENY" always;
  add_header Referrer-Policy "strict-origin-when-cross-origin" always;
  add_header Permissions-Policy "geolocation=(), microphone=(), camera=()" always;

  ssl_certificate     /etc/letsencrypt/live/orochiverse.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/orochiverse.com/privkey.pem;
}
```

```bash
sudo install -d /var/www/orochiverse
sudo install -m 644 index.html /var/www/orochiverse/index.html
sudo nginx -t && sudo systemctl reload nginx
```

## Custom domain (any host)

DNS records at the registrar:

| Type   | Name | Value                          |
|--------|------|--------------------------------|
| `A`    | `@`  | (host's IPv4 — see their docs) |
| `AAAA` | `@`  | (host's IPv6 — optional)       |
| `CNAME`| `www`| `orochiverse.com.`             |

Most managed hosts (Netlify, Vercel, Cloudflare Pages) will give you the
exact records to paste under their "Custom domain" UI and provision the TLS
cert automatically.

## Cache strategy

`index.html` changes whenever the site is edited. Recommended headers:

```
Cache-Control: public, max-age=300, must-revalidate
```

Five minutes lets a CDN absorb traffic spikes without stranding visitors on a
stale build for hours after a deploy. Don't set `immutable` on `index.html`.

## Smoke test before you call it done

In a browser, with the deployed URL open:

- [ ] Hero canvas animates (eight strands flowing into the right-side core).
- [ ] Headline shimmer cycles every ~7.5s. Caret blinks.
- [ ] Marquee scrolls left without a visible seam.
- [ ] Sticky nav goes from transparent → bordered when you scroll past 40px.
- [ ] Capability cards tint blue on hover. Tags shift to accent ring.
- [ ] Contact form: empty submit shows toast; bad email shows toast; valid
      submit opens the OS mail client with the draft pre-filled.
- [ ] Mobile (≤780px): nav hamburger opens a fullscreen menu; tapping a link
      closes it.
- [ ] Lighthouse: Performance ≥ 90, Accessibility ≥ 95, Best Practices ≥ 95.

If Lighthouse flags **render-blocking fonts**, swap the Google Fonts `<link>`
for a self-hosted `@font-face` (`fonts.bunny.net` is a drop-in privacy-
preserving alternative if GDPR matters in your context).
