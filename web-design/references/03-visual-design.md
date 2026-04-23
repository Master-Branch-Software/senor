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

### Contrast: WCAG 2.2 and APCA

WCAG 2.2 contrast is based on relative luminance and is the standard recognized by accessibility law (ADA, EAA, Section 508, EN 301 549). Targets:

- 4.5:1 for body text and 3:1 for large text (≥ 18 pt or 14 pt bold).
- 3:1 for non-text UI components and focus indicators.

APCA (Accessible Perceptual Contrast Algorithm) is perceptually weighted and often more predictive of real readability, especially for dark-mode text and mid-lightness hues. It scores with `Lc` values:

- Lc 90+ for fluent reading body text.
- Lc 75+ for small text.
- Lc 60+ for content text down to 16 px.
- Lc 45+ for large text (24 px+).
- Lc 30+ for non-text UI, large headings.

Important context on APCA's status: APCA was exploratory content in earlier WCAG 3 drafts, was pulled from the drafts during the 2023 rework, and is not currently part of any approved W3C standard. WCAG 3 itself remains in draft and its contrast algorithm is still to be determined. APCA's own compliance documentation prohibits any claim of "WCAG 3 compliance."

The practical rule:

- Always verify WCAG 2.2 AA for legal compliance and as the primary threshold.
- Use APCA as a secondary, design-time sanity check when WCAG gives results that feel wrong (common around mid-lightness blues and oranges, and in dark mode).
- If a color pair fails WCAG 2.2 but passes APCA, document the decision and accept the legal risk; do not substitute APCA for WCAG 2.2 in audit reports or compliance statements.

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

## Dark mode

Dark mode is no longer a power-user preference — between 55% and 82% of users have it enabled on at least one device. A website that forces bright white on a dark-mode user creates an uncomfortable, jarring experience, especially in low light. Treat dark mode as a parallel design system, not a CSS inversion filter.

### The core principle: dark does not mean black

Pure `#000000` against pure `#FFFFFF` creates extreme contrast (21:1) that causes visual fatigue and a halation effect where text appears to blur. Most professional dark interfaces use dark gray as the base:

- Spotify: `#121212`
- VS Code: `#1E1E1E`
- Twitter/X: `#15202B`
- Material Design baseline: `#121212`

A subtle tonal quality — slightly warm, slightly cool, or slightly blue-tinted — makes dark interfaces feel premium rather than simply inverted.

### Elevation through lightness, not shadows

In light mode, depth is communicated through shadows. In dark mode, shadows are nearly invisible against dark backgrounds. Instead, use progressively lighter surface colors to indicate elevation:

```css path=null start=null
[data-theme="dark"] {
  --color-bg:         #121212; /* base page */
  --color-surface-1:  #1e1e1e; /* cards, dialogs */
  --color-surface-2:  #232323; /* raised cards */
  --color-surface-3:  #2c2c2c; /* modals, popovers */
}
```

Each level is slightly lighter. The visual rule: higher elevation = lighter surface.

### Color adaptation

Colors that work on white backgrounds often feel aggressive on dark backgrounds. Reduce saturation by 10–20% and increase lightness slightly:

| Role | Light mode | Dark mode |
| --- | --- | --- |
| Primary blue | `#1976D2` | `#64B5F6` |
| Success green | `#22C55E` | `#4ADE80` |
| Warning amber | `#F59E0B` | `#FCD34D` |
| Error red | `#EF4444` | `#F87171` |
| Primary text | `#1D1729` | `rgba(255,255,255,0.87)` |
| Secondary text | `#6B7280` | `rgba(255,255,255,0.60)` |
| Disabled text | `#9CA3AF` | `rgba(255,255,255,0.38)` |

Do not use pure white (`#FFFFFF`) for body text — use `rgba(255,255,255,0.87)` or `#ECECEC`. Pure white is too harsh and causes halation.

### Images and media

- Photographs generally work well in dark mode without modification.
- Illustrations with white backgrounds create jarring bright rectangles. Either provide dark-mode-specific versions or apply a subtle backdrop.
- SVG icons should use `fill: currentColor` so they inherit the text color and adapt automatically.
- Logos that are pure black on white need a light version for dark mode:

```html path=null start=null
<picture>
  <source srcset="/logo-dark.svg" media="(prefers-color-scheme: dark)" />
  <img src="/logo-light.svg" alt="Company name" />
</picture>
```

For photography, a subtle brightness reduction integrates the image with the dark UI: `filter: brightness(0.85)` in dark mode. Use this carefully — it should not be perceptible as a desaturation, only as a slightly less jarring brightness.

### CSS implementation

```css path=null start=null
/* Define tokens for both modes */
:root {
  --bg: #ffffff;
  --surface: #f5f7fa;
  --text: #1d1729;
  --text-secondary: #6b7280;
  --border: #e5e7eb;
  --primary: #1976d2;
}

/* System preference */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg:            #121212;
    --surface:       #1e1e1e;
    --text:          rgba(255, 255, 255, 0.87);
    --text-secondary: rgba(255, 255, 255, 0.60);
    --border:        rgba(255, 255, 255, 0.12);
    --primary:       #64b5f6;
  }
}

/* Explicit override */
[data-theme="dark"] {
  --bg:            #121212;
  --surface:       #1e1e1e;
  --text:          rgba(255, 255, 255, 0.87);
  --text-secondary: rgba(255, 255, 255, 0.60);
  --border:        rgba(255, 255, 255, 0.12);
  --primary:       #64b5f6;
}
```

```js path=null start=null
// Prevent flash of wrong theme on page load
// Place this inline script in <head>, before any content
(function () {
  const saved = localStorage.getItem('theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  if (saved === 'dark' || (!saved && prefersDark)) {
    document.documentElement.setAttribute('data-theme', 'dark');
  }
})();
```

Always detect and apply the theme before the first paint to prevent the "flash of incorrect theme" — a white flash on a dark-mode page is one of the most jarring UX failures in dark mode implementation.

### Typography in dark mode

- Text appears heavier on dark backgrounds due to the irradiation illusion. Consider a slightly lighter font weight for body text in dark mode.
- Slightly increase letter-spacing for improved readability on dark backgrounds.
- Links must remain distinguishable from body text — pair color with underline, as color contrast alone is harder to perceive against dark surfaces.

### Dark mode anti-patterns

- Pure black background (`#000000`) — use dark gray instead.
- Automatic color inversion (`filter: invert(1)`) — photos become negatives, colors shift unpredictably.
- Forgetting shadows — light-mode shadows disappear on dark backgrounds. Use lighter surface colors for elevation instead.
- Inconsistent text colors — mixing pure white with off-whites looks sloppy.
- Only testing in mockups — test on a real phone in a dark room.
- Treating dark mode as an afterthought — design both modes in parallel from the beginning.

## Feel targets

Set one word for each project and audit against it. Examples:

- "Sharp" — high contrast, narrow gutters, tight motion, terse copy.
- "Warm" — rounded corners, amber accents, generous leading, contractions in copy.
- "Calm" — low chroma, generous spacing, slow easings, short paragraphs.
- "Confident" — large type, long headings, bold primary color, direct CTAs.
- "Playful" — variable weight shifts, springy motion, conversational copy, illustrative accents.
