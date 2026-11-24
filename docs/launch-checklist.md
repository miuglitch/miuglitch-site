# Website launch checklist: Miu (miuglitch)

Status key: **[x] done** (built into `/site`), **[ ] to-do** (needs you, the host, or the live domain), **n/a** (doesn't apply to a one-page static site).

The site files are at the root of this repo, and this checklist is in `docs/`.

> **Domain placeholder:** the site assumes `https://miuglitch.com/`. If the real domain is different, search and replace `https://miuglitch.com` in `index.html`, `robots.txt` and `sitemap.xml` (6 places total).

---

## 1. Content and copy

- [x] Headline "I'M MIU, I MAKE MUSIC AND PLUGINS" above the social links
- [x] All five social links up front in the first screen: Spotify, Bandcamp, Instagram, YouTube, SoundCloud
- [x] Tracking parameter (`?si=…`) removed from the Spotify link
- [x] Copyright line in the footer (© 2026)
- [ ] Proofread everything one last time on a phone
- [ ] Click every link on the live site and make sure each one opens the right profile
- [ ] Optional: add a plugins section or link (shop, Gumroad, KVR, etc.) once there's a URL for it
- [ ] Optional: a contact or booking email (also needed for `security.txt`, see section 6)

## 2. Error pages

- [x] Custom `404.html` in the site's style, with a link back home and `noindex`
- [x] `.htaccess` points Apache at the 404 page (`ErrorDocument 404 /404.html`)
- [ ] After deploying, open a made-up URL (e.g. `/does-not-exist`) and confirm you see the custom 404 **and** that the server returns status 404, not 200. Netlify, Cloudflare Pages, GitHub Pages and Vercel pick up `404.html` automatically.
- n/a 500 / maintenance page (no server code to fail)

## 3. Favicons and app icons

Follows the modern minimal set (one `.ico`, one SVG, one Apple touch icon, a web manifest with 192 and 512 icons).

- [x] `favicon.ico` (16, 32, 48 px), made from the M in the logo, red on near-black
- [x] `favicon.svg` (scalable, crisp on high-DPI tabs)
- [x] `apple-touch-icon.png` (180×180, full MIU logo, solid background so iOS doesn't add black corners)
- [x] `icons/icon-192.png`, `icons/icon-512.png` and a padded `icon-maskable-512.png` for Android
- [x] `site.webmanifest` with name, colors and icons
- [x] `theme-color` meta tag (colors the mobile browser bar)
- [ ] Check the tab icon in Chrome, Safari and Firefox after deploy, and "Add to Home Screen" on an iPhone and an Android phone

## 4. SEO and meta tags

- [x] `<html lang="en">`, `charset`, `viewport`
- [x] Unique `<title>` and `meta description`
- [x] `rel="canonical"`
- [x] One `<h1>`, then `<h2>` for sections
- [x] Descriptive `alt` text on every content image (decorative images use `alt=""`)
- [x] Structured data (JSON-LD `MusicGroup` with `sameAs` links to all profiles) so search engines can connect the site to the profiles
- [x] `rel="me"` on social links (lets platforms like Mastodon verify the site; harmless elsewhere)
- [x] `robots.txt` allowing everything and pointing to the sitemap
- [x] `sitemap.xml`
- [ ] Replace the `miuglitch.com` placeholder if the domain differs
- [ ] Add the site to **Google Search Console** and **Bing Webmaster Tools**, verify ownership, submit `sitemap.xml`
- [ ] Put the website link in the bio of every profile (Instagram, YouTube, SoundCloud, Bandcamp, Spotify for Artists). Links back from your own profiles help search engines trust the site.
- [ ] Pick one host form (`miuglitch.com` **or** `www.miuglitch.com`) and redirect the other to it. The `.htaccess` sends `www` to the bare domain.

## 5. Social sharing (Open Graph and X/Twitter cards)

- [x] `og:title`, `og:description`, `og:type`, `og:url`, `og:site_name`, `og:locale`
- [x] `og:image` at 1200×630 (`og-image.jpg`, from the red "Stop being logical" art) with width, height and alt text
- [x] `twitter:card = summary_large_image` plus title, description, image and alt
- [ ] After deploy, test the preview with the Facebook Sharing Debugger, the LinkedIn Post Inspector, and by pasting the link into Discord, iMessage or WhatsApp. These tools cache old previews, so use their "scrape again" button after changes.

## 6. Security

- [x] No JavaScript, no forms, no third-party scripts, so very little to attack
- [x] Fonts are self-hosted, so the site makes no requests to Google or other third parties (also better for privacy rules like GDPR)
- [x] Security headers ready in two formats: `_headers` (Netlify and Cloudflare Pages) and `.htaccess` (Apache):
  - `Strict-Transport-Security` (forces HTTPS for a year)
  - `Content-Security-Policy` (only loads files from your own domain; blocks scripts entirely)
  - `X-Content-Type-Options: nosniff`
  - `Referrer-Policy: strict-origin-when-cross-origin`
  - `Permissions-Policy` (turns off camera, mic, location, etc.)
  - `X-Frame-Options: DENY` and CSP `frame-ancestors 'none'` (stops other sites framing yours)
  - `Cross-Origin-Opener-Policy: same-origin`
- [x] No inline styles or scripts, so the strict CSP works without `unsafe-inline`
- [ ] If the host is something else (GitHub Pages can't set headers; Vercel uses `vercel.json`), copy the header values into that host's settings, or put Cloudflare in front of it
- [ ] Scan the live site at securityheaders.com and Mozilla HTTP Observatory, aim for A or better
- [ ] Turn on two-factor login for the domain registrar and the hosting account
- [ ] Turn on registrar lock / transfer lock and WHOIS privacy for the domain
- [ ] Optional: `/.well-known/security.txt` with a contact email (skipped because there's no public email yet)
- [ ] Optional: once HTTPS has been stable for a while, add `; preload` to HSTS and submit to hstspreload.org (hard to undo, so only when sure)
- n/a CSRF, SQL injection, login security, cookie flags (no forms, backend or cookies)

## 7. HTTPS and domain

- [ ] Domain registered and DNS pointed at the host
- [ ] HTTPS certificate active (free and automatic on Netlify, Cloudflare Pages, GitHub Pages and Vercel; Let's Encrypt elsewhere)
- [ ] `http://` redirects to `https://` (handled in `.htaccess` for Apache; a toggle on most other hosts)
- [ ] Check there are no mixed-content warnings (all site URLs already use relative or `https://` paths)
- [ ] Optional: CAA DNS record limiting which authorities can issue certificates
- [ ] Domain auto-renew is on, with a card that won't expire soon

## 8. Accessibility

- [x] "Skip to links" link for keyboard users
- [x] Visible focus outline (cyan) on every link
- [x] Social links are real `<a>` elements inside a labelled `<nav>`, icons hidden from screen readers, text names visible
- [x] Color contrast: body text and link labels are light on near-black (well above WCAG AA 4.5:1); red is used for accents, not for small body text
- [x] Respects `prefers-reduced-motion` (turns off glitch, flicker and smooth scroll)
- [x] Touch targets are much larger than 44×44 px
- [x] Text uses `rem` sizes, so it scales with the browser's font setting; works zoomed to 200%
- [x] No horizontal scrolling at 390 px phone width (tested)
- [ ] Quick screen-reader pass (VoiceOver on iPhone or Mac: swipe through the page)
- [ ] Run Lighthouse or axe DevTools on the live site and fix anything it flags

## 9. Performance

- [x] Images resized and converted to WebP with JPEG fallback, multiple sizes with `srcset` so phones get small files (about 80 MB of original photos became roughly 0.9 MB for a full desktop visit and 0.4 MB on a phone)
- [x] Width and height on every image (prevents layout jumps)
- [x] Below-the-fold images `loading="lazy"`; the hero image is preloaded with `fetchpriority="high"`
- [x] Only three small font files (latin subset, WOFF2), with `font-display: swap` and the headline font preloaded
- [x] No JavaScript at all
- [x] Long cache headers for images and fonts (in `_headers` and `.htaccess`), gzip for text in `.htaccess`
- [ ] Run PageSpeed Insights on the live URL (target: green Core Web Vitals on mobile)
- [ ] If you later replace an image with a new one of the **same name**, rename it (e.g. `hero-1600-v2.webp`), since images are cached for a year

## 10. Analytics and monitoring

- [ ] Choose analytics. For a site like this, a privacy-friendly one is simplest because it needs no cookie banner: **Plausible**, **Fathom**, **Umami**, **Cloudflare Web Analytics** or **Netlify Analytics** (the last two need no code at all). If you add a script, add its domain to the CSP `script-src` and `connect-src`.
- [ ] Google Analytics would need a cookie consent banner in the EU/EEA and a privacy policy; not recommended for this page
- [ ] Uptime monitor (UptimeRobot or Better Stack, free tiers) with alerts by email
- [ ] Optional: Spotify, YouTube and SoundCloud links with UTM tags if you want to see which ones people click

## 11. Legal and privacy

- [x] No cookies, no tracking, no third-party requests, so no cookie banner is needed as built
- [ ] If you add analytics that use cookies, or a newsletter form, add a privacy policy page and consent handling
- [ ] Confirm you have the rights to every photo (photographer credit or license). The two-person TV-wall photo shows someone else, so make sure they're happy to be on it.
- [x] **Skipped the headshot with the video game characters** ("MiuHeadshot_large.png"). It shows Mario, Pikachu, Sonic, Mega Man, Kirby and others, which are trademarked characters, so using it as site art could draw a takedown.
- [x] Font licenses: Chakra Petch and Share Tech Mono are under the SIL Open Font License (free for websites); license files are in `/site/fonts`. Platform icons are from Simple Icons (CC0).
- n/a Terms of service, imprint (an *Impressum* is legally required for business sites in Germany and Austria; add one if you sell plugins to customers there)

## 12. Cross-browser and device testing

- [x] Chromium desktop (1440 px) and phone (390 px) screenshots checked
- [ ] Safari on iPhone, Chrome on Android
- [ ] Firefox and Safari on desktop
- [ ] Rotate the phone to landscape once

## 13. Deploy and after launch

- [ ] Upload everything inside `/site` to the web root (keep the folder structure; include the hidden `.htaccess` file on Apache hosts)
- [ ] Recommended free hosts: **Cloudflare Pages** or **Netlify** (drag and drop the folder; `_headers` works there with no changes)
- [ ] Keep a backup copy of the site folder (or put it in a GitHub repo for version history)
- [ ] Re-run this checklist's "after deploy" checks: 404 status, HTTPS redirect, headers scan, OG preview, PageSpeed
- [ ] Update `lastmod` in `sitemap.xml` when the page changes

---

## Files in `/site`

| File | Purpose |
|---|---|
| `index.html` | The page |
| `404.html` | Not-found page |
| `styles.css` | All styling (no JS) |
| `logo.svg` | Red MIU logo from your SVG |
| `favicon.ico`, `favicon.svg`, `apple-touch-icon.png`, `icons/`, `site.webmanifest` | Icon set |
| `og-image.jpg` | 1200×630 share image |
| `robots.txt`, `sitemap.xml` | Search engines |
| `_headers`, `.htaccess` | Security, caching, redirects (use whichever matches your host) |
| `img/` | Optimized photos (WebP and JPEG, two or three sizes each) |
| `fonts/` | Self-hosted fonts and their licenses |

## Sources

- [OWASP HTTP Headers Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html) and [OWASP Secure Headers Project](https://owasp.org/www-project-secure-headers/)
- [Evil Martians: How to Favicon (minimal modern set)](https://evilmartians.com/chronicles/how-to-favicon-in-six-files-that-fit-most-needs)
- [specification.website: favicons](https://specification.website/spec/foundations/favicons/)
- [Smashing Magazine: essential checks before launching](https://smashingmagazine.com/2009/04/15-essential-checks-before-launching-your-website)
- [hungrysandwichclub/web-launch-checklist](https://www.github.com/hungrysandwichclub/web-launch-checklist)
- [Website launch checklist (gist)](https://gist.github.com/chrisgherbert/38470f43e2b1ac0cbb230afd82e84c8f)
- [paulund.co.uk: website go-live checklist](https://paulund.co.uk/website-go-live-checklist)
