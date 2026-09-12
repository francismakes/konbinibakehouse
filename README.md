# Handoff: Konbini Bakehouse — single-screen landing page

## Overview
A one-screen, full-viewport landing page for Konbini Bakehouse. It does exactly two things: present the logo as a winking face, and give visitors one way to get in touch (a `mailto:` to sales@konbinibakehouse.com). No nav, no scroll, no sections below the fold.

## About the design files
The files in `reference/` are **design references created in HTML** — a prototype of the intended look and behavior, not production code to lift wholesale. The task is to **recreate this design in the target codebase's environment** (React/Next, Vue, Astro, SwiftUI, plain HTML — whatever is already in use), following its established conventions for components, styling, and asset handling. If no codebase exists yet, pick the simplest thing that ships a static single-page site (e.g. Astro or a plain static page + Tailwind) and implement it there.

- `reference/konbini-landing.html` — standalone, opens in any browser. This is the canonical visual + motion reference.
- `reference/Konbini Landing.dc.html` — the authoring source it was exported from (same markup; ignore if unfamiliar).
- `assets/konbini-logo.svg` — the supplied brand logo (arched KONBINI wordmark + the two "eye" shapes). **Use this file's path data verbatim**; do not redraw.
- `assets/brand-board.png` — brand exploration board the palette, type and ornaments were derived from.

## Fidelity
**High-fidelity.** Colors, type, spacing, and motion below are final. Recreate pixel-for-pixel using the codebase's existing primitives where they exist.

## Screen: Hero / contact (the only screen)

### Layout
Single `<main>`, full viewport:

- `min-height: 100svh` (use `svh`, not `vh` — mobile browser chrome), `box-sizing: border-box`, `overflow: hidden`, `position: relative`
- `display: flex; flex-direction: column; align-items: center; justify-content: center`
- `gap: clamp(20px, 4.6vh, 64px)`
- `padding: clamp(24px, 5vh, 56px) clamp(20px, 6vw, 64px) clamp(54px, 8vh, 84px)` (extra bottom padding reserves room for the absolutely positioned product line)
- `background: #292a2d`; default text color `#f0e7dc`
- Content children sit at `z-index: 2`; ornaments are at default stacking (behind)

Everything must fit inside one viewport at both 924×540 (short desktop pane) and 390×844 (mobile) — no scrollbar. That is what the `vh`-based clamps and the logo's `max-height` are for. Do not swap them for fixed px.

### Content stack (top → bottom, centered)
1. **Logo lockup** — column, `gap: clamp(24px, 4.4vh, 46px)`, `width: 100%`, `max-width: 640px`
   - Logo SVG: `viewBox="0 0 702.9 510.55"`, `fill: #fd5a13`, `width: 100%`, `max-width: min(560px, 74vw)`, `max-height: min(406px, 34vh)`, `height: auto`, `overflow: visible`, `role="img" aria-label="Konbini"`. The height cap is load-bearing — without it the mark pushes the CTA off short screens.
   - `BAKEHOUSE` wordmark: Poppins 500, `font-size: clamp(13px, 2.6vw, 22px)`, `letter-spacing: .42em` **plus matching `text-indent: .42em`** (so the trailing letter-space doesn't break optical centering), `line-height: 1`, color `#f0e7dc`, uppercase literal text.
2. **Tagline block** — column, `gap: clamp(10px, 2vh, 22px)`
   - Tangerine rule: `44 × 4 px`, `border-radius: 2px`, `background: #fd5a13`
   - `SIMPLE TREATS.` / `BRIGHTER DAYS.` on two lines (hard `<br>`): Poppins 500, `clamp(14px, 3.2vw, 20px)`, `letter-spacing: .22em` + `text-indent: .22em`, `line-height: 1.5`, `#f0e7dc`
3. **Contact block** — column, `gap: 14px`, `width: 100%`, `max-width: 440px`
   - Label: `FOR INQUIRIES, CONTACT` — Poppins 600, `11px`, `letter-spacing: .2em` + `text-indent: .2em`, uppercase, `#f0e7dc` at `opacity: .7`
   - Field: an `<a href="mailto:sales@konbinibakehouse.com">` styled as an input pill. `display: flex; align-items: center; justify-content: space-between; gap: 12px`, `width: 100%`, `min-height: 60px` (touch target), `padding: 10px 10px 10px clamp(16px, 4vw, 24px)`, `border: 1.5px solid rgba(240,231,220,.3)`, `border-radius: 999px`, `background: rgba(240,231,220,.06)`, text `#f0e7dc` Poppins 500 `clamp(13px, 3.3vw, 16px)`.
     - Left: `sales@konbinibakehouse.com`, `white-space: nowrap; overflow: hidden; text-overflow: ellipsis`
     - Right: `40 × 40` circle, `background: #fd5a13`, glyph `→` (`&#8594;`) at `15px` in `#292a2d`
     - Hover: `border-color: #fd5a13`, `box-shadow: 0 12px 30px rgba(253,90,19,.24)`, `transform: translateY(-1px)`; `transition: border-color .2s, box-shadow .2s, transform .2s`
     - Focus: give it a visible ring (`outline: 2px solid #fd5a13; outline-offset: 2px`) — the prototype relies on the UA default, production should not.
4. **Product line** (absolutely positioned, not in flex flow) — `COOKIES / MUFFINS / SCONES / BARS / CINNAMON ROLLS`: `position: absolute; bottom: clamp(16px, 3vh, 28px); left: 50%; transform: translateX(-50%)`, full width with `padding: 0 20px`, centered, Poppins 500 `9.5px`, `letter-spacing: .18em` + `text-indent: .18em`, uppercase, `#f0e7dc` at `opacity: .75`, `line-height: 1.7`.

### Decorative ornaments
Six shapes derived from the packaging pattern on `assets/brand-board.png`, all `aria-hidden="true"`, all bleeding off the canvas edges, all sized in `vmin` so they scale with the viewport and never crowd the centered content. Exact geometry is in `reference/konbini-landing.html` — copy the SVG source rather than redrawing.

| Shape | Position | Size | Colors |
|---|---|---|---|
| Cookie (disc + 4 chips) | `top: -6vmin; left: -7vmin` | `34vmin` | disc `#fd5a13`, chips `#292a2d` |
| Muffin (flat-bottomed cloud top + tapered base with 3 charcoal gaps) | `top: 4vmin; right: -3vmin` | `26vmin` | `#9ecfa9` on `#292a2d` |
| Cinnamon swirl (bare spiral stroke, **no enclosing disc**) | `bottom: -5vmin; right: -6vmin` | `32vmin` | stroke `#f266a3`, `stroke-width: 6.5` (of a 100-unit viewBox), `stroke-linecap: round` |
| Biscuit (rounded rect + 3 slots), rotated `-18deg` | `bottom: 6vmin; left: -4vmin` | `20vmin` | `#a595ef`, slots `#292a2d` |
| Matcha disc | `top: 50%; left: -9vmin` | `20vmin` | `#9ecfa9` |
| Tangerine dash | `top: 56%; right: 5vmin` | `6vmin × 1.6vmin` | `#fd5a13` |

Keep ornaments out of the logo's eye band (top ~10% of the content column) — a stray dash there reads as a third eye — and out from under the product line, where cream text over matcha fails contrast.

## Interactions & behavior

### The wink (the point of the page)
The logo's two eyes are the `circle` (left, `cx=211.07 cy=45.94 r=45.94`) and the `rect` (right, `x=410.79 y=29.15 w=117.91 h=33.58`) inside the logo SVG.

- The **circle eye never moves.**
- The **rect eye animates**: dash → full circle → dash. The dash state is the wink (eye closed); the circle state is the eye open.
- Implemented by animating SVG **geometry properties** in CSS (supported in Chrome/Safari/Firefox; verify in your target browsers, and see fallback below):

```css
@keyframes kb-wink {
  0%,38%   { x:410.79px; y:29.15px; width:117.91px; height:33.58px; rx:0px; }
  47%,60%  { x:423.8px;  y:0px;     width:91.88px;  height:91.88px; rx:45.94px; }
  70%,100% { x:410.79px; y:29.15px; width:117.91px; height:33.58px; rx:0px; }
}
/* on the rect */
animation: kb-wink 4.6s cubic-bezier(.5,0,.4,1) infinite;
```
The open state is a `91.88px` square with `rx = 45.94` — i.e. a circle the exact size of the left eye — centered on the dash's own center (`469.74, 45.94`).

Fallback if geometry-property animation is unavailable in a required browser: swap between two sibling shapes (rect + circle) with `opacity`/`visibility`, or use SMIL `<animate attributeName="width" …>`. Do not fake it with `transform: scaleY()` — that squashes the shape's stroke-free proportions wrongly and was explicitly rejected.

### Entrance
`kb-rise` — `opacity 0 → 1`, `translateY(14px) → 0`, `.7s ease both`. Staggered: logo lockup `0s`, tagline `.12s`, contact block `.22s`.

### Ornament drift
`kb-float` — `translateY(0 → -10px → 0)`, `ease-in-out`, `infinite`, with deliberately mismatched durations so they never sync: cookie `9s/0s`, swirl `10s/.4s`, muffin `11s/.8s`, biscuit `12s/1.2s`. The matcha disc and tangerine dash are static.

### Motion accessibility
Wrap all three animations in a `@media (prefers-reduced-motion: reduce)` opt-out. With motion reduced, render the rect eye in its **dash** state (the resting brand lockup) and skip the drift and the entrance offsets.

### Click behavior
The pill is a real anchor — `mailto:sales@konbinibakehouse.com`. No JS, no form, no validation. If the product later wants a captured email instead of a mail client, that is a new design, not a variant of this one.

### Responsive
No breakpoints. Everything is `clamp()` / `vmin` / `svh` fluid, so one implementation covers 320px phones to large desktop. Verify at 320×568, 390×844, 768×1024, 924×540, 1440×900, 1920×1080 — the acceptance criterion at every size is: no scrollbar, and logo + BAKEHOUSE + tagline + contact field all visible.

## State management
None. Static page, zero state, zero data fetching.

## Design tokens

### Color
| Token | Hex | Use |
|---|---|---|
| Charcoal | `#292a2d` | page background, knockouts inside ornaments, arrow glyph |
| Tangerine | `#fd5a13` | logo fill, rule, arrow circle, hover accent, cookie |
| Cream | `#f0e7dc` | all text |
| Raspberry | `#f266a3` | cinnamon swirl |
| Lilac | `#a595ef` | biscuit |
| Matcha | `#9ecfa9` | muffin, matcha disc |
| Cream 30% | `rgba(240,231,220,.3)` | field border |
| Cream 6% | `rgba(240,231,220,.06)` | field fill |
| Tangerine 24% | `rgba(253,90,19,.24)` | field hover shadow |

Palette names and intent come from `assets/brand-board.png`. Text opacities in use: `1` (BAKEHOUSE, tagline, field), `.75` (product line), `.7` (inquiries label).

### Typography
**Poppins** (Google Fonts, weights 400/500/600/700), fallbacks `"Century Gothic", system-ui, sans-serif`. It stands in for the geometric sans on the brand board — if the brand has a licensed display face (the board's wordmark looks custom/Futura-adjacent), substitute it and keep the letter-spacing values.

| Role | Size | Weight | Tracking |
|---|---|---|---|
| BAKEHOUSE | `clamp(13px, 2.6vw, 22px)` | 500 | `.42em` |
| Tagline | `clamp(14px, 3.2vw, 20px)` | 500 | `.22em` |
| Field text | `clamp(13px, 3.3vw, 16px)` | 500 | normal |
| Inquiries label | `11px` | 600 | `.2em` |
| Product line | `9.5px` | 500 | `.18em` |

Every tracked (letter-spaced) run carries a `text-indent` equal to its `letter-spacing` to stay optically centered.

### Spacing / radius / shadow
- Section gaps: `clamp(20px, 4.6vh, 64px)` (main), `clamp(24px, 4.4vh, 46px)` (logo→BAKEHOUSE), `clamp(10px, 2vh, 22px)` (tagline), `14px` (contact)
- Radii: `999px` (field), `50%` (arrow, discs), `2px` (rule), `7px` (biscuit, in viewBox units)
- Only shadow in the design: `0 12px 30px rgba(253,90,19,.24)` on field hover

## Assets
- `assets/konbini-logo.svg` — client-supplied logo, used verbatim (inlined into the page so the eye can be animated; keep it inline, not an `<img>`).
- `assets/brand-board.png` — reference only, not shipped. Source of the palette and the ornament vocabulary.
- Ornaments are hand-authored SVG in the page — no icon library, no raster images, no external requests other than the webfont.
- No image generation was involved; all shapes are code.

## Files in this bundle
```
design_handoff_konbini_landing/
├── README.md
├── assets/
│   ├── brand-board.png
│   └── konbini-logo.svg
└── reference/
    ├── konbini-landing.html      ← open this
    └── Konbini Landing.dc.html   ← authoring source
```

## Acceptance checklist
- [ ] Logo SVG path data matches `assets/konbini-logo.svg` exactly
- [ ] `BAKEHOUSE` present under the mark (it is **not** in the supplied SVG)
- [ ] Right-hand dash eye morphs dash → circle → dash; left dot eye static
- [ ] Contact pill fires `mailto:sales@konbinibakehouse.com`, has a visible focus ring, ≥44px tall
- [ ] No vertical scroll at 320×568, 390×844, and 924×540
- [ ] `prefers-reduced-motion` disables the wink, drift, and entrance
