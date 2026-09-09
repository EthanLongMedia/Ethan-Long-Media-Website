---
name: elm-work-card
description: Add or edit a program in the "Who I shoot for" section of the Ethan Long Media site — Northwood, REAL Basketball, and any new team or client. Use when a new client is signed, a logo needs swapping, or the work grid needs reordering.
---

# Ethan Long Media — the work grid

Section `#work`, "Who I shoot for". Three cards in `.work-grid`.

## The card shape

```html
<article class="card" data-tilt>
  <div class="card-top">
    <span class="card-idx">01</span>
    <span class="card-kind">NCAA D-II · Midland, MI</span>
  </div>
  <div class="card-vis"><img src="assets/brand/LOGO.png" alt="… logo"></div>
  <div class="card-body">
    <h3 class="card-name">Northwood<em>University</em></h3>
    <p class="card-pitch">…</p>
    <span class="card-go">Gameday film <i></i></span>
  </div>
</article>
```

Six things to change per card: `card-idx`, `card-kind`, the logo `src` + `alt`,
`card-name`, `card-pitch`, `card-go`.

## Rules the existing cards follow

- **`card-idx` is sequential and zero-padded** — `01`, `02`, `03`. Renumber the
  whole grid when inserting; do not append `04` in the middle.
- **It numbers within its own section.** `#proof` and `#training` also use
  `.card` blocks with their own `card-idx`, so an `02` there is unrelated. Do
  not "fix" it to be globally unique.
- **`card-name` splits across two lines** with `<em>` on the second word:
  `Northwood<em>University</em>`, `REAL<em>Basketball</em>`. Not decorative —
  the CSS sizes off it.
- **`card-kind` is the tier, not the sport**: `NCAA D-II · Midland, MI`,
  `Travel program · 17U`, `Player development`.
- **`card-pitch` names a specific piece of work**, not a service category —
  *"the overtime cut against Lake Erie that broke a million views."* One
  sentence. If you cannot name a real cut, the client is not ready for a card.
- **`card-go`** is the work type: `Gameday film`, `Exposure edits`. Leave the
  empty `<i></i>` — CSS draws the arrow.

## Logos

`assets/brand/` — PNG with the background keyed out to transparency. The
`-clean` suffix marks it: `northwood-clean.png`, `real-clean.png`. A logo with a
white box behind it shows as a white box on the dark card. Key it out before
committing and keep the naming.

## Card 03 has no logo

It uses an inline SVG of Michigan, projected from real GeoJSON boundary data and
simplified — not traced. The `viewBox` is cropped to the artwork's own bounds,
with a dot on the Lower Peninsula's area centroid. If you replace that card with
a logo client, delete the SVG block. Do not try to re-fit the `viewBox` by eye.

## `data-tilt` is required

That attribute is what the script hooks for the tilt effect, and the reveal
animation is orchestrated per card — each swings up on its own hinge with the
violet rule drawing across the top. A card without `data-tilt` sits dead next to
the others.
