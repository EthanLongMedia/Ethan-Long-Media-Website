# Ethan Long Media

Portfolio site for a basketball videographer in mid-Michigan. Single static page,
no build step, no dependencies to install — open `index.html` and it runs.

**Live:** _(add your URL once deployed)_

---

## What's in it

- **Particle wordmark hero** — the ETHAN LONG / MEDIA lockup is sampled from real
  type onto a canvas, then flown in as ~5,000 particles that settle into the
  letterforms. Renders at 60fps.
- **Proof section** — the viral Northwood v Lake Erie cut sitting next to the
  numbers it earned (1.2M Instagram, 934.8K TikTok), with counters that run on scroll.
- **Work cards** — Northwood, REAL Basketball, and Michigan trainers. The Michigan
  outline is real boundary data, projected from GeoJSON and simplified, not traced.
- **Pricing, guarantee, process** — three tiers, a free-recut guarantee, three steps.
- **Booking form** — composes a pre-filled email. Falls back to a copyable panel
  where `mailto:` is blocked.

## Running it locally

Any static server will do. With Python:

```bash
python3 -m http.server 4321
```

Then open <http://localhost:4321>.

Opening `index.html` directly via `file://` mostly works, but the video and the
webfont behave better over HTTP.

## Structure

```
index.html          the whole site — markup, styles and behaviour
robots.txt
assets/
  brand/            client logos, backgrounds keyed out to transparency
  social/og.png     1200x630 link-preview card
  vendor/           GSAP + ScrollTrigger + Lenis, vendored so there's no CDN
  video/            the reel and its poster frame
```

Everything lives in `index.html` on purpose. At this size a build step would cost
more than it saves, and the whole site can be dropped on any host as-is.

## Deploying

**Netlify** — drag this folder onto <https://app.netlify.com/drop>. Live in about
a minute, custom domains supported.

**GitHub Pages** — Settings → Pages → Deploy from branch → `main` → `/ (root)`.
The `.nojekyll` file stops Jekyll from stripping anything.

**Anywhere else** — it's static files. Upload them.

### After a domain is live

Change the two `og:image` paths in `index.html` from relative to absolute:

```html
<meta property="og:image" content="https://yourdomain.com/assets/social/og.png">
<meta name="twitter:image" content="https://yourdomain.com/assets/social/og.png">
```

Link previews on iMessage, Instagram and X will not resolve a relative image, so
until this is done a shared link shows no card.

## Editing the common things

| What | Where |
|---|---|
| Prices | `.price-grid` — three `.tier` blocks |
| Stats | `data-count` attributes in the proof section |
| Turnaround | search `72 hours` |
| Email | search `ethanlongmedia@gmail.com` |
| Accent colour | `--violet` in `:root` |
| The reel | replace `assets/video/viral.mp4` and `poster.jpg` |

### Replacing the video

Keep it short and encode it properly — basketball is high-motion and starves at
low bitrates, which shows up as smearing a second or two in. Around 3000 kbps at
roughly 560px wide, with a keyframe every second, holds up well. Regenerate
`poster.jpg` from a frame of whatever you use.

## Browser support

Modern evergreen browsers. Uses `aspect-ratio`, `IntersectionObserver`, canvas 2D
and `prefers-reduced-motion`. No IE, no polyfills.
