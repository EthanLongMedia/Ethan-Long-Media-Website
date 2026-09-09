---
name: elm-pricing
description: Edit the packages, rates, and guarantee on the Ethan Long Media site. Use when rates change for a new season, when a package is added or dropped, or when what is included in a tier changes.
---

# Ethan Long Media — pricing

Section `#pricing` ("No DMs to find the price"), then `#guarantee`.

## Current tiers — three, in this order

| Order | Tier | Rate | Classes |
|---|---|---|---|
| 1 | Single game | `$50` / game | `tier rv` |
| 2 | Single training | `$75` / session | `tier rv` |
| 3 | **Full season** | `$500` / season | `tier feature value rv` |

Exactly one tier carries `feature`. It is currently **Full season** — the
season deal is the thing to sell, so it is highlighted and placed last, after
the two single rates have set the anchor.

## The season maths must stay consistent

```html
<p class="tier-sub">Up to 15 games — $33 a game, a third off the single-game rate.</p>
```

Three numbers in one sentence, all derived: `$500 ÷ 15 = $33`, and `$33` against
`$50` is a third off. **Change any of the season price, the game cap, or the
single-game rate and all three have to be recomputed.** A season price that does
not divide into the claimed per-game rate is the most visible possible error on
a pricing page.

## Tier markup

```html
<div class="tier rv">
  <h3>Single game</h3>
  <div class="amt">$50<s>/ game</s></div>
  <div class="tier-groups">
    …
  </div>
</div>
```

- `.amt` — the number, unit inside `<s>`. The `<s>` is styled, not
  strikethrough. Keep `/ game`, `/ session`, `/ season` lowercase with the
  leading slash.
- `.tier-groups` wraps the bullet groups. Match the grouping of neighbouring
  tiers — a tier with ungrouped bullets sits visually wrong next to two that
  have them.
- `.tier-sub` is the qualifier line, feature tier only.
- **`.rv` on every tier** — the reveal-on-scroll hook. Omit it and the card
  never appears.
- Buttons link to `#contact` (the form in `#finale`), never a bare mailto.

## Writing the bullets

Each answers an objection, in this order of argument: what is covered → how it
is shot (gimbal, angles — the anti-"shaky phone footage" line) → what gets cut →
the grade → deliverable formats (vertical for Reels/TikTok, horizontal for the
rest) → real audio off an external mic → **turnaround**.

Do not add a bullet that is a feature with no objection behind it.

## Turnaround appears in five places

```bash
grep -n '72 hours' index.html
```

It is in the game bullets, the training bullets, the `#process` steps, and the
process summary. Changing the promise means changing all of them.

## The standing promise

*"Shot, graded and cut by me — no second shooter, no outsourced edit. Travel
inside mid-Michigan is included."* If you bring in a second shooter or start
charging travel, that line changes before the prices do.
