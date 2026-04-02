# Brand Identity Reference â€” Salva Navarro / PKI Tools

> This file is the authoritative brand reference for this repository.
> Use these tokens, URLs and guidelines in any UI, document, or deliverable created here.

## Brand Kit (CDN)

**Reference page:** https://salva-navarro.github.io/brand-kit/
**Full asset pack download:** https://salva-navarro.github.io/brand-kit/PKI_Tools_SuperPack_AI_v2.zip

## Color Tokens

| Token | Value | Use |
|---|---|---|
| `--brand-primary` | `#2F4F9F` | Main blue â€” headers, buttons, links |
| `--brand-primary-tint` | `#4B70C9` | Hover states, secondary actions |
| `--brand-primary-shade` | `#1E3470` | Pressed states, dark headers |
| `--brand-secondary` | `#D4A017` | Gold accent â€” highlights, badges |
| `--brand-secondary-tint` | `#E8BC45` | Light gold, hover accents |
| `--brand-secondary-shade` | `#A07C10` | Dark gold, pressed accents |
| `--brand-dark` | `#1C1C1C` | Body text, dark backgrounds |
| `--brand-light` | `#FFFFFF` | Backgrounds, cards |

### CSS Usage

```css
@import url('https://salva-navarro.github.io/brand-kit/02_COLOR_PALETTES/palette_01_primary.json');

:root {
  --brand-primary:   #2F4F9F;
  --brand-secondary: #D4A017;
  --brand-dark:      #1C1C1C;
  --brand-light:     #FFFFFF;
}
```

## Typography

| Role | Font | Weight |
|---|---|---|
| Headings | Montserrat | 600â€“700 |
| Body | Inter | 400â€“500 |
| Monospace / Code | Consolas | 400 |

```html
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700&family=Inter:wght@400;500&display=swap" rel="stylesheet">
```

## Logo Assets (CDN)

```html
<!-- Modern style (recommended) -->
<img src="https://salva-navarro.github.io/brand-kit/01_LOGO_VARIATIONS/style_modern/logo_001.svg" />

<!-- Classic style -->
<img src="https://salva-navarro.github.io/brand-kit/01_LOGO_VARIATIONS/style_classic/logo_001.svg" />

<!-- Minimalist -->
<img src="https://salva-navarro.github.io/brand-kit/01_LOGO_VARIATIONS/style_minimalist/logo_001.svg" />
```

## Icon Set (CDN)

```html
<!-- Certificate icon -->
<img src="https://salva-navarro.github.io/brand-kit/05_ICON_SETS/icon_413d_certificate.svg" width="32" />

<!-- Shield -->
<img src="https://salva-navarro.github.io/brand-kit/05_ICON_SETS/icon_423d_shield.svg" width="32" />

<!-- Key -->
<img src="https://salva-navarro.github.io/brand-kit/05_ICON_SETS/icon_433d_lock.svg" width="32" />
```

## Color Palette JSON (fetch)

```js
const palette = await fetch(
  'https://salva-navarro.github.io/brand-kit/02_COLOR_PALETTES/palette_01_primary.json'
).then(r => r.json());
// { name: "primary", primary: "#2F4F9F", secondary: "#D4A017", dark: "#1C1C1C", light: "#FFFFFF" }
```

## Evaluation Criteria (AI Selection)

```js
const evalCriteria = await fetch(
  'https://salva-navarro.github.io/brand-kit/07_EVALUATION_DATA/evaluation_structure.json'
).then(r => r.json());
// 5 criteria: Visual Hierarchy 20%, Scalability 15%, Uniqueness 25%, Tech Quality 20%, Versatility 20%
```

