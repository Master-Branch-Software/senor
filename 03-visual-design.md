# Visual Design

Visual design is applied information theory. Every visual choice either clarifies hierarchy and intent or dilutes it.

## Hierarchy before style

Before choosing fonts or colors, rank the page's content from most to least important. The most important element becomes the visual anchor; everything else is sized and styled to defer to it. When every heading is bold and every button is primary, nothing is primary.

Tools for creating hierarchy, in order of how often they are used:

1. Size — the headline is larger than the body.
2. Weight — primary text is heavier than supporting text.
3. Color — active and primary elements use the brand's most saturated color; passive and supporting elements use neutrals.
4. Spacing — white space around an element amplifies it.
5. Position — readers scan top-to-bottom and start-to-end; place the anchor early.
6. Contrast — high contrast advances, low contrast recedes.
7. Motion — moving items draw the eye; use sparingly.

Combine two or three, not all seven. Over-amplification reads as noise.

## The safe rules (Anthony Hobday)

A compact list of defaults that almost never fail on their own. Each can be broken deliberately, but only by someone who knows why.

- Use near-black and near-white instead of pure `#000` and `#FFF`. Softening both ends reduces eye strain.
- Saturate neutrals toward the brand color instead of using flat grays. Cooler or warmer neutrals feel intentional.
- Optical alignment beats mathematical alignment. Align what looks aligned, not what calculates to aligned (triangles, quotation marks, counterweighted glyphs).
- Use a 4-, 8-, or 12-column grid for layout; 12 is the most flexible.
- Nest corners: outer radii should be larger than inner radii, with the offset equal to the padding (`outer = inner + padding`).
- Outer padding should be greater than or equal to inner padding.
- Body text is at least 16 px.
- Line length sits around 60–75 characters (`max-inline-size: 70ch`).
- Pair a serif with a sans-serif, or use two weights of the same family. Avoid two unrelated serifs or two unrelated sans-serifs.
- The same color in two places creates relationship. Use intentionally, not accidentally.

## Spacing

Spacing is not padding; it is information. Work from a single scale that everything on the site uses.

An 8-pt base scale is the most common default. In design-system terms, expose tokens such as `space-1` (4 px), `space-2` (8 px), `space-3` (12 px), `space-4` (16 px), `space-6` (24 px), `space-8` (32 px), `space-12` (48 px), `space-16` (64 px), `space-24` (96 px). Every margin, padding, and gap should come from the scale.

For fluid spacing that scales with viewport or container, derive values with `clamp()` or Utopia-generated steps. Never use raw `vw` for spacing on typography; you will lose the user's zoom preference.

Rules of thumb:

- Related items share a tighter gap than unrelated items (Law of Proximity applied literally).
- A section should have visibly more vertical space between it and the next section than between its internal elements.
- Pages have "outer padding" that matches or exceeds any component's "inner padding."
- Respect a minimum clickable target of 44 × 44 CSS pixels on touch devices.

## Grid and layout

A 12-column grid with consistent gutters is the most flexible default for marketing pages; a 4-column grid is typical at mobile widths. Define the grid once, enforce it everywhere.

Modern CSS Grid makes grids fluid:

```css path=null start=null
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 20rem), 1fr));
  gap: clamp(1rem, 2vw, 2rem);
}
```

Use named grid lines or subgrid when related components must share alignment across levels.

## Typography

### Choosing type

- Use at most two typefaces, unless an explicit reason demands more. A super-family (e.g. Inter + Inter Display; Söhne + Söhne Mono) gives variety without clashing.
- Choose a workhorse body face first (legible at small sizes, good at 16 px, multiple weights). Then pick a display face only if the body face cannot carry headings alone.
- For UI and body text, prefer humanist or neo-grotesque sans-serifs at 16–18 px with 1.5 line height.
- For long-form reading, serif body faces often improve readability, particularly in the 16–20 px range.
- Match x-height, stroke contrast, and feeling. A modern body face pairs poorly with a Victorian display.

### Variable fonts

Variable fonts expose axes (weight, width, slant, optical size, custom axes) in a single file. Benefits:

- One HTTP request instead of many.
- Smooth interpolation for responsive or animated weight.
- The `opsz` optical-size axis adjusts letterforms at different sizes for optimum legibility.

Self-host WOFF2 files and reference them with `@font-face`. Use `font-display: swap` for text-heavy faces; `font-display: optional` for decorative ones to avoid CLS.

### Type scales

Derive sizes from a modular scale. A 1.125, 1.200, or 1.250 ratio is common for body-driven sites; 1.333 or 1.414 for editorial; 1.500+ for bold display-heavy work. Compute once, reuse forever.

Example fluid scale (two-step derivation with Utopia or manual `clamp`):

```css path=null start=null
:root {
  --step--1: clamp(0.83rem, 0.80rem + 0.20vw, 0.94rem);
  --step-0:  clamp(1.00rem, 0.95rem + 0.25vw, 1.13rem);
  --step-1:  clamp(1.20rem, 1.13rem + 0.38vw, 1.41rem);
  --step-2:  clamp(1.44rem, 1.33rem + 0.56vw, 1.76rem);
  --step-3:  clamp(1.73rem, 1.57rem + 0.81vw, 2.20rem);
  --step-4:  clamp(2.07rem, 1.85rem + 1.13vw, 2.75rem);
  --step-5:  clamp(2.49rem, 2.17rem + 1.56vw, 3.44rem);
}
```

### Line length and line height

- Body copy: 60–75 characters per line (`max-inline-size: 65ch` is a safe default).
- Line height: 1.5 for body; 1.1–1.25 for display; 1.3–1.4 for UI labels.
- Avoid multi-column text on the web unless truly motivated; scanning across columns is costly on narrow screens.

### Pairing rules

- Contrast is the variable: opposite categories pair better than near-siblings. A neo-grotesque sans with a transitional serif works; two humanist sans-serifs fight.
- Keep the vertical rhythm aligned: line heights should feel harmonious across faces.
- Limit to two families; use weights and sizes for variation.

### Text rendering

- Use `text-wrap: balance` for headings so lines split aesthetically.
- Use `text-wrap: pretty` for long-form paragraphs to reduce orphans and widows.
- Use `hanging-punctuation: first allow-end last` for refined text blocks where supported.
- Set `font-feature-settings` for numerals (`"tnum"` for tabular figures in data-heavy UI).

## Color

### Working color space

Use OKLCH for authoring and interpolation. Compared to HSL:

- Perceptually uniform: a 10-unit change in lightness looks the same whether at 20% or 80%.
- Same chroma across hues reads as equivalent saturation.
- Interpolation across hues stays in gamut and avoids muddy midpoints.

Syntax: `oklch(L C H)` where `L` is perceived lightness (0 to 1), `C` is chroma (roughly 0 to 0.4), `H` is hue in degrees.

### Contrast: WCAG vs APCA

WCAG 2.2 contrast is based on relative luminance and is the current legal standard. Targets:

- 4.5:1 for body text and 3:1 for large text (≥ 18 pt or 14 pt bold).
- 3:1 for non-text UI components and focus indicators.

APCA (Accessible Perceptual Contrast Algorithm) is perceptually weighted and more predictive of real readability. It scores with `Lc` values:

- Lc 90+ for fluent reading body text.
- Lc 75+ for small text.
- Lc 60+ for content text down to 16 px.
- Lc 45+ for large text (24 px+).
- Lc 30+ for non-text UI, large headings.

Verify WCAG AA for legal compliance. Use APCA as a sanity check when WCAG gives results that look wrong (common around mid-lightness blues and oranges).

### Palette construction

A professional palette has more steps than "primary, secondary, accent." Adopt a 12-step model:

- Steps 1–2: Page and component backgrounds.
- Steps 3–5: Subtle UI (hover, focus, pressed) on those backgrounds.
- Steps 6–8: Borders and separators.
- Steps 9–10: Solid backgrounds for primary UI (buttons, badges); step 9 is the accessible default.
- Step 11: Accessible foreground text for low-contrast backgrounds.
- Step 12: High-contrast foreground text.

Radix Colors is a reference implementation. The 60-30-10 rule is a coarser default for marketing pages: 60% dominant neutral, 30% secondary, 10% accent.

Always ship light and dark palettes. Use `color-scheme: light dark` on `html` and the `light-dark()` function for scheme-sensitive tokens. Avoid inverting colors algorithmically; hand-tune dark values so contrast and temperature hold.

### Tools

- `oklch.com` — OKLCH picker and live contrast visualisation.
- `huemint.com` — AI-generated palettes constrained to a mood or brand.
- `coolors.co` — palette generation and export.
- `realtimecolors.com` — apply a palette to a real layout for rapid evaluation.
- `leonardocolor.io` (Adobe Leonardo) — accessibility-first palette generation with target contrast ratios.
- `contrast.tools` and `myndex.com/APCA` — contrast audits.
- Radix Colors (`radix-ui.com/colors`) — ready-made 12-step scales in light and dark.
- Tailwind's default palette — a decent neutral and saturated-color reference.

## Imagery

- Prefer real, specific, brand-owned imagery over generic stock.
- Screenshots should feature real content, not Lorem Ipsum or dummy data.
- Treat imagery as content: crop, color-grade, and position deliberately.
- Use SVG for logos and icons, AVIF/WebP for photography, WebP with JPG fallback when wider compatibility is required.
- Ensure decorative images have `alt=""` and that meaningful images have descriptive `alt` text.

## Iconography

- Use one icon system, not four. Tabler, Lucide, Phosphor, and Heroicons each provide extensive, consistent sets.
- Optical size matches text size. A 16-px icon should feel similar in weight to 16-px text.
- Align icons to the baseline of adjacent text.
- Icons without labels are rarely understood. Always pair icons with text, except for universally understood glyphs (close X, search, hamburger) inside appropriate contexts.

## Depth

- Shadows represent distance from a surface, not weight. Use a consistent shadow scale (e.g. `shadow-sm`, `shadow`, `shadow-md`, `shadow-lg`) that corresponds to elevation.
- Prefer multi-layer shadows (ambient + direct) over single shadows for realism.
- Borders, tints, and blurs are also depth cues; do not stack all three on the same element.
- Glassmorphism works only with genuine backdrop content behind the surface and readable foreground contrast.

## Design styles

Each of the dominant modern web styles has a coherent visual grammar. Pick one primary style per project; drift mid-site reads as inconsistency.

### Swiss / International Typographic

Grid-driven, generous whitespace, sans-serif dominant, asymmetric balance, limited color. Defaults well for editorial and high-trust corporate sites.

### Minimalism

Maximum content-to-chrome ratio. One strong accent color, reserved typography, strict spacing scale. Works for products where the product is the star (Apple, Linear, Stripe).

### Maximalism

Dense layering, vivid color, expressive type, intentional tension. Effective for creative agencies and brands with a strong voice. Demands craft; sloppy maximalism reads as chaos.

### Brutalism / neo-brutalism

Raw edges, harsh contrasts, monospace type, visible grids, oversized forms. Signals authenticity or rebellion; in excess signals amateurism.

### Editorial

Serifs, hang-lines, generous leading, magazine-like compositions. Ideal for content-driven sites (journalism, long-form marketing, portfolios).

### Glassmorphism and neumorphism

Useful accents; poor all-page strategies. Contrast is hard to preserve, and they date quickly.

## Feel targets

Set one word for each project and audit against it. Examples:

- "Sharp" — high contrast, narrow gutters, tight motion, terse copy.
- "Warm" — rounded corners, amber accents, generous leading, contractions in copy.
- "Calm" — low chroma, generous spacing, slow easings, short paragraphs.
- "Confident" — large type, long headings, bold primary color, direct CTAs.
- "Playful" — variable weight shifts, springy motion, conversational copy, illustrative accents.
