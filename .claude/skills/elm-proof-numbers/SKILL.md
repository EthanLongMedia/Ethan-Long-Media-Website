---
name: elm-proof-numbers
description: Update the view counts, engagement stats, and camera-kit line on the Ethan Long Media site. Use when a reel puts up new numbers, when the Instagram/TikTok totals change, or when new camera gear needs to show up on the site.
---

# Ethan Long Media — the numbers

One file: `index.html`. No build step.

## The four stats are animated counters, not plain text

In `#proof`, inside `.stat-grid`. Each one is **split between an attribute and
the markup** — editing only the visible text does nothing, because the script
overwrites it on scroll.

```html
<div class="num"><span data-count="1.2" data-dec="1">0.0</span><s>M</s></div>
<div class="lb">Instagram views</div>
<div class="sub">Single reel · @northwood_mbb</div>
```

| `data-count` | `data-dec` | Suffix `<s>` | Label |
|---|---|---|---|
| `1.2` | 1 | `M` | Instagram views |
| `934.8` | 1 | `K` | TikTok views |
| `246` | 0 | `K` | Combined likes |
| `6349` | 0 | *(none)* | Saves |

Rules:

- **`data-count` is the real number.** The text inside the span is only the
  starting frame (`0.0` / `0`) — the counter replaces it.
- **`data-dec`** is decimal places, and must match: `1.2` needs `data-dec="1"`,
  or it animates to `1`.
- **The unit lives in `<s>`**, outside the span. Never put `M` or `K` in
  `data-count` — `parseFloat` will silently take the number and drop the letter.
- **`data-comma`** (boolean, on Saves only) runs the value through
  `toLocaleString('en-US')` → `6,349`. Use it for anything ≥ 1000 with no
  K/M suffix.

The counter fires once, at `top 88%`, over 2.1s. Reload and scroll to see it —
it will not re-run.

## The `.sub` line has to stay true to the number

`Combined likes` shows `246K` with a sub of `106K Instagram · 140.6K TikTok`.
That adds up. If you change one, fix the arithmetic — this section is called
"The receipts," and a total that does not match its own breakdown is the fastest
way to lose it.

## The same numbers also appear as plain text

```bash
grep -n '1\.2M\|934\.8K' index.html
```

| Where | Note |
|---|---|
| `<meta name="description">` | |
| `og:description` | |
| `og:image:alt` | carries the numbers too — easy to miss |
| `twitter:description` | |
| `.hero-meta` | `<b>1.2M / 934.8K views</b>` |
| `#proof` `.sec-note` | the "one cut, two algorithms" paragraph |

**Changing a stat means editing both the `data-count` and every plain-text
copy.** And if the social card art shows the numbers, regenerate it — see
`elm-social-card`.

## Keep the framing, not just the digits

The claim is *one cut, two algorithms, same result — that's not a fluke, that's
a formula.* If the two platform numbers stop being close, that sentence needs
rewriting, not just renumbering.

## The kit line

`.hero-meta` right column: `ZV-E10 II · RS 3 Mini · External mic`. Three items,
middot-separated, shortest useful name each. It is a credibility signal, not a
spec sheet — do not let it grow to four.

## Rounding

Match what the platform shows. `1.2M`, not `1,200,000`. TikTok's `934.8K` keeps
one decimal because that is how TikTok displays it.
