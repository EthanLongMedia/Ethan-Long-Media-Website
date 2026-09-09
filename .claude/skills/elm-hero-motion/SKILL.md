---
name: elm-hero-motion
description: Work on the Ethan Long Media motion — the canvas particle wordmark, GSAP/ScrollTrigger reveals, the scroll-materialized nav, Lenis smooth scroll, and the inline videos. Use when animation misbehaves, when the script silently stops running, or before touching anything in the inline script block.
---

# Ethan Long Media — motion

The inline `<script>` at line ~1107 of `index.html`, plus three vendored
libraries in `assets/vendor/`: `gsap.min.js`, `ScrollTrigger.min.js`,
`lenis.min.js`. **Vendored on purpose — no CDN.** The site must run from a
folder with no network.

## Read this before editing the script

The whole thing is wrapped in an IIFE for a reason:

> The minified vendor bundles leak bare globals (one of them declares `W`), and
> a top-level `let W` here would collide and kill this whole script at parse
> time.

A parse-time collision means **the entire page's motion silently does nothing** —
no error you will spot on a casual reload, just a dead page. Keep new variables
inside the IIFE. Single-letter names are the trap: `W`, `H`, `P`, `N` are
already taken.

## The particle wordmark

`#particles` canvas. The ETHAN LONG / MEDIA lockup is sampled from real type
rendered offscreen, then flown in as ~5,000 particles that settle into the
letterforms.

- **`DPR` is capped at 2** — `Math.min(devicePixelRatio || 1, 2)`. Uncapping it
  kills phones.
- **Supersampled at `S = 2`.** "MEDIA" is small type; sampling at CSS resolution
  starves it of points. The same offscreen canvas is reused as the crisp solid
  layer once the particles land — do not create a second one.
- **Size is clamped against height as well as width**:
  `clamp(Math.min(ow * 0.098, oh * 0.145), 60, 260)`. The height term is not
  redundant — width-only sizing let the wordmark collide with the tagline on a
  wide, short window.
- `drawTracked()` does manual letter-spacing by measuring each character,
  because canvas has no `letter-spacing`.

## Text builders

`.hero-tag .ln` is built from `data-line`, one `<span class="ch">` per
character — **spaces included**, so span index maps straight onto string index.
`data-em` highlights a word by finding its index in that string. Change how
spaces are handled and the `data-em` lookup breaks.

`#finLine` splits `data-words` on `|` and hard-codes `i===2` for the underlined
word. Reordering the words moves the underline.

## The stat counters

`[data-count]` elements animate on `ScrollTrigger.create({ start:'top 88%',
once:true })` over 2.1s with `power2.out`. `once:true` means **they do not
re-run** — reload to test. Data contract is in `elm-proof-numbers`.

## Scroll

- `history.scrollRestoration = 'manual'` + `scrollTo(0,0)` at the top — the hero
  must start from the top.
- **`.rv` is the reveal class** ScrollTrigger hooks. Any new element that should
  animate in needs it.
- The **nav materializes** once you are clear of the hero, riding the existing
  Lenis scroll callback. Do not add a second scroll listener that fights it.
- Lenis drives smooth scroll; ScrollTrigger is wired to it.

## Video

Two inline videos: the reel in `#proof` and the training cut in `#training`.

- **iOS pauses inline video on its own** under memory pressure, so there is
  recovery code that restarts it. Keep it.
- Both carry `?v=` cache-busting on `src` and `poster`
  (`training.mp4?v=2`). **Bump the number when you replace a file** or hosts
  and browsers keep serving the cached one.
- `<video>` is a known source of scroll jank on iOS — that is why the existing
  attributes are set the way they are. Do not add autoplay behaviour casually.

### Re-encoding a video

Basketball is high-motion and starves at low bitrates — it shows as smearing a
second or two in. **~3000 kbps at roughly 560px wide, keyframe every second.**
Regenerate the poster from a frame of whatever you use.

## Reduced motion

`REDUCED` is read once at the top and branched on throughout — eight places.
Every animation has a plain-fade fallback landing on the **same resting state**
(see the `FIN_WORDS` pair: reduced gets opacity-only, full gets slide-up +
rotate). Any animation you add needs the same treatment.

Press feedback is separate and always on: `.btn:active` compresses to
`scale(.96)` at `.1s`, because a tap should confirm itself even with motion off.

## Checking it

Hard-reload with the console open. A blank hero with no error is the `W`
collision. Test at a wide-and-short window — that is where the wordmark sizing
has broken before. Then test with reduced motion on, and on a real iPhone for
the video.
