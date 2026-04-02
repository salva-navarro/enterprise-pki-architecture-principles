# Brand Identity - Salva Navarro

**Kit de marca corporativa para todos los proyectos PKI Tools / SnPKI IdentiCert Suite.**

---

## Archivos Canonicos (Fuente de Verdad)

| Archivo | URL | Uso |
|---------|-----|-----|
| **brand-tokens.css** | https://salva-navarro.github.io/brand-kit/brand-tokens.css | CSS custom properties - importar en cualquier stylesheet |
| **brand-tokens.json** | https://salva-navarro.github.io/brand-kit/brand-tokens.json | Design tokens completos - para IA, tooling, Figma plugins |
| **Visual reference** | https://salva-navarro.github.io/brand-kit/ | Pagina de referencia visual |
| **Asset pack** | https://salva-navarro.github.io/brand-kit/PKI_Tools_SuperPack_AI_v2.zip | 393 assets: logos, iconos, patrones |

---

## Instruccion para IA (Copilot / ChatGPT / Cursor)

Pega esto en cualquier prompt para que la IA use exactamente esta marca:

\\\`nUsa los design tokens de la marca Salva Navarro desde:
  CSS:  https://salva-navarro.github.io/brand-kit/brand-tokens.css
  JSON: https://salva-navarro.github.io/brand-kit/brand-tokens.json

Siempre importa brand-tokens.css y usa var(--color-primary), var(--gradient-hero), etc.
Nunca uses valores hex directamente. Tipografia: Montserrat (titulares) + Inter (cuerpo).
\\\`n
---

## Importar en un proyecto

\\\css
@import url('https://salva-navarro.github.io/brand-kit/brand-tokens.css');
\\\`n
---

## Colores Principales

| Token CSS | Valor | Uso |
|-----------|-------|-----|
| --color-primary | #2F4F9F | PKI Blue - botones, enlaces, headings |
| --color-secondary | #D4A017 | Heritage Gold - badges, CTAs, acentos |
| --color-dark | #1C1C1C | Texto principal |
| --color-light | #FFFFFF | Fondos |

---

## Tipografia

- **Heading**: Montserrat (var(--font-heading))
- **Body**: Inter (var(--font-body))
- **Mono**: JetBrains Mono (var(--font-mono))

---

*Brand System v2.0 - Actualizado 2026-04-02*
