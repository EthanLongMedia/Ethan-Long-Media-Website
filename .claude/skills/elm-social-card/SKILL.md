---
name: elm-social-card
description: Regenerate the Ethan Long Media link-preview card and get shared links to show it. Use when the social preview is wrong, stale, or missing, when the stats on the card change, or when a shared link shows no image.
---

# Ethan Long Media — the social card

The domain is live: **ethanlongmedia.com**, via `CNAME`, served by GitHub Pages
(`.nojekyll` stops Jekyll stripping files). The old go-live checklist is done —
canonical, absolute OG URLs and typed image meta are all in place. What is left
is keeping the card correct.

## Current state in `<head>`

```html
<link rel="canonical" href="https://ethanlongmedia.com/">
<meta property="og:url"   content="https://ethanlongmedia.com/">
<meta property="og:image" content="https://ethanlongmedia.com/assets/social/og.jpg?v=3">
<meta property="og:image:secure_url" content="…/og.jpg?v=3">
<meta property="og:image:type"  content="image/jpeg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:image:alt"   content="… 1.2M on Instagram and 934.8K on TikTok">
<meta name="twitter:image"      content="…/og.jpg?v=3">
```

## The `?v=` bump is the whole trick

> Bump `?v=` when the card changes, otherwise scrapers keep serving the version
> they already cached.

Facebook, iMessage, X and LinkedIn each cache independently and aggressively.
**Replacing `og.jpg` without bumping `?v=` changes nothing anyone can see.**

It appears in **three places** — `og:image`, `og:image:secure_url`, and
`twitter:image`. All three must move together. Currently `v=3`.

## Regenerating the card

The source lives in `tools/share-card/`:

```
card.html    the 1200x630 card, laid out in HTML/CSS
shot.jpg     the source photo of the shooter
```

Open `card.html` (it is fixed at exactly 1200×630) and screenshot it at
`deviceScaleFactor: 1`. Export as JPEG to `assets/social/og.jpg`.

The layout is tuned to this specific photo and the comments explain each number:
the shooter sits ~66px from the source frame's left edge, so the blend has to
happen inside that margin; the photo starts right of centre because he is on the
left of the frame; the scrim is a soft shoulder only, not a full-width wash, or
it flattens the violet glow. **A different photo means re-tuning `left`, `width`
and the mask stop — not just swapping `shot.jpg`.**

If the stats on the card change, update `og:image:alt` to match.

## After regenerating

1. Replace `assets/social/og.jpg`.
2. Bump `?v=` in all three meta tags.
3. Push, wait for Pages to deploy.
4. Re-share into iMessage, an Instagram DM, and X, and confirm the card renders.

A preview cached under the old URL will not clear on its own — the version bump
is what forces a re-scrape.

## Still outstanding

**`robots.txt` has no sitemap.** The line is still commented out even though the
domain went live:

```
# Sitemap: https://ethanlongmedia.com/sitemap.xml
```

Uncomment it **only after `sitemap.xml` actually exists** — pointing at a 404 is
worse than pointing at nothing. For a one-page site the sitemap is a single
`<url>` entry.

**`README.md` is stale** — it still says `assets/social/og.png` and tells you to
convert relative OG paths to absolute. Both are already done, and the file is
`.jpg`. Fix it when you next touch the README.
