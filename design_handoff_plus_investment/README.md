# Handoff: Plus Investment — Landing page

Premium real-estate landing for **Plus Investment**, a Turkey-based real-estate agency. Russian-language site (RU/EN/TR planned) targeting investors, relocators and holiday buyers.

---

## About the Design Files

The HTML files in this bundle (`design/index.html`, `design/design-system.html`) are **design references** — fully styled prototypes that show the intended look, motion and behavior of the site. They are **not** production code to ship as-is.

Your task is to **recreate these designs in the target codebase's environment** (React/Next.js, Vue/Nuxt, Astro, plain HTML/Eleventy, etc.) following its established patterns, component model and tooling. If no codebase exists yet, pick the framework best suited to the project — for a marketing/SEO landing of this kind, **Next.js (App Router) with TypeScript** or **Astro** are good defaults.

The prototypes use vanilla HTML/CSS/JS with no build step, so all values, animations and JS logic can be lifted directly and translated into your component model.

---

## Fidelity

**High-fidelity.** Pixel-perfect mockups with final colors, typography, spacing, copy, interactions, scroll-effects and responsive breakpoints. Recreate pixel-for-pixel using the target codebase's component library and styling approach. All exact hex values, font sizes, easings and timings are encoded in the HTML and listed below in **Design Tokens**.

---

## Site Map

Single long-scroll landing page composed of 13 blocks, plus a style-guide page:

| # | Block | Section ID | Notes |
|---|---|---|---|
| 01 | Header (sticky) | `#topbar` | Sticky top-bar with utility row + main nav. Becomes translucent + blurred when pinned. |
| 02 | Hero | `.hero` | Full-viewport, background image of Alanya, single CTA → modal lead form. |
| — | Ticker | `.ticker` | Marquee of city names, dark band between hero and search. |
| 03 | Search | `#search` | Filter card with primary fields + "Расширенный поиск" expandable panel + Search by ID. |
| 04 | Popular properties | `#popular` | 3-column grid of property cards. |
| 04b | Developer showcase | `#showcase` | **Pinned horizontal scroll** with parallax, 3 large complex cards. |
| 05 | SEO categories | `#seo` | 4-tile mosaic with background images. |
| 06 | Lead form (Получить подборку) | `#lead` | Dark editorial section with full form (name, phone, email, comment). |
| 07 | Why us | `#why` | **Pinned horizontal scroll** with 6 cards, animated SVG wave background. |
| 08 | Video | `#video` | YouTube-style hub: feature 16:9 + 2 vertical shorts + 3 horizontal cards + channel CTA. Filter tabs. |
| 09 | Testimonials | `#testimonials` | Slider, 3 per view, text + video reviews, "Читать полностью" expand. |
| 10 | Blog | `#blog` | 3 article cards with hover-reveal CTA. |
| 11 | FAQ + Ask form | `#faq` | Accordion + inline ask form (dark gold-accent block). |
| 12 | SEO text | `#seo-text` | Editorial article with sticky TOC scrollspy. |
| 13 | Footer (reveal) | `.site-footer` | **Fixed footer**, page scrolls "over" it via rounded-corner reveal effect. |

A separate **Style Guide page** (`design/design-system.html`) documents the full token system: color swatches, type scale, buttons, components, principles.

---

## Design Tokens

All tokens are declared as CSS custom properties at `:root` in `index.html`. Port them to your design-token source of truth (Tailwind config, CSS vars in global stylesheet, Figma tokens, etc.).

### Colors

| Token | Hex | Usage |
|---|---|---|
| `--ink` | `#15171b` | Primary text, dark surfaces, primary button |
| `--ink-2` | `#222429` | Dark surface (lead block, ask form, footer, pre-footer) |
| `--ink-3` | `#2c2f36` | Tertiary dark |
| `--paper` | `#eef0f2` | Default page background |
| `--paper-2` | `#e6e8eb` | Secondary surfaces, chip backgrounds |
| `--paper-3` | `#dadde1` | Tertiary surfaces, image placeholders |
| `--mute` | `#9aa0a6` | Tertiary text, captions |
| `--text` | `#1a1c20` | Body text on light surfaces |
| `--text-soft` | `#54585f` | Secondary text on light surfaces |
| `--gold` | `#f5ca3a` | Brand accent, primary CTA, hover states |
| `--gold-deep` | `#e3b524` | Darker variant of gold |

### Typography

| Family | Weights loaded | Usage |
|---|---|---|
| **Inter** (Google Fonts) | 200, 300, 400, 500, 600, 700, 800 | All body, headings, UI |
| **JetBrains Mono** (Google Fonts) | 400, 500 | Eyebrows, tags, monospace labels, numbering, timestamps |

CSS vars: `--sans: "Inter", ui-sans-serif, system-ui, ...`, `--mono: "JetBrains Mono", ui-monospace, ...`.

**Display heading pattern** used in every section title:
- Font: Inter, **weight 300** (thin/light), letter-spacing `-0.025em` to `-0.035em`, line-height ~1.02
- `clamp(36px, 4.4vw, 60px)` for section H2, `clamp(44px, 6.6vw, 104px)` for hero H1
- Inside the heading, wrap the emphasized phrase in `<em>` — styled as `font-style:normal; font-weight:500-600`. This creates the signature "light + bold" mixed-weight feel.
- A tertiary color accent (`color: var(--gold)`) is sometimes used on a sub-phrase (`.accent` class)

**Eyebrow pattern** (above every section title):
```html
<div class="sec-eyebrow">Section name</div>
```
Renders as: 11px JetBrains Mono, letter-spacing `.22em`, uppercase, `var(--text-soft)`, preceded by a 24×1px gold horizontal line via `::before`.

**Body text**: 15-16px, line-height 1.55-1.65, `font-weight: 300`, max-width ~60ch.

### Radii

| Token | Value | Usage |
|---|---|---|
| `--radius-s` | `10px` | Small inputs, internal elements |
| `--radius-m` | `16px` | Property cards, SEO tiles, blog cards |
| `--radius-l` | `22px` | Search card, modal, large content cards |
| (ad-hoc) | `24px` | Lead form, footer reveal corners, ask form |
| (ad-hoc) | `999px` | All pills, buttons, badges, chips |

### Container & Spacing

- `--container: 1320px` max page width (1280px on style-guide page)
- `--pad: clamp(20px, 4vw, 56px)` horizontal page padding
- Section vertical padding: `clamp(80px, 8vw, 128px)` (`.section`) or `clamp(48px, 5vw, 80px)` (`.section--tight`)

### Shadows

Used sparingly. Two recurring patterns:

- **Card lift on hover**: `0 30px 60px -30px rgba(15,17,22,.25)` with `transform: translateY(-4px)`
- **Search card resting**: `0 30px 80px -40px rgba(15,17,22,.25)`
- **Gold CTA lift**: `0 14px 40px -16px rgba(245,202,58,.6)` on `.lb__submit:hover`, `.ask__submit:hover`

### Border conventions

- Light cards: `1px solid rgba(20,22,26,.06)` resting → `rgba(20,22,26,.12)` on active
- Dark cards: `1px solid rgba(255,255,255,.08)` resting → `rgba(255,255,255,.14)` on active
- Dashed mono inputs: `1px dashed rgba(20,22,26,.2)` (search id, ranges)

---

## Components

### Button system
Located at `.btn` in `index.html`. Variants stacked via modifiers (`.btn--gold`, `.btn--ghost`, `.btn--dark`, `.btn--lg`). Every primary CTA contains an inline circular arrow chip (`.arrow`) that translates right on hover.

```html
<a class="btn btn--gold">Заказать звонок <span class="arrow">→</span></a>
```

### Form fields
Two main treatments:

1. **Floating labels** (used in lead form modal, lead block, ask form): label sits inside input baseline, shrinks/translates to top on focus or filled state.
2. **Stacked labels** (used in search filter, footer subscribe): label always above input as mono uppercase 10-11px caption.

Validation: red border `#ff6b6b` + label color change + `.err` span shown on `.field.is-error`. JS validators in `index.html` cover `phone` (`/^[\+\d\s\-\(\)]{8,}$/`) and `email` (standard regex).

### Badge / chip system
- `.badge` — white pill, 11px JetBrains Mono uppercase
- `.badge--gold` — yellow
- `.badge--dark` — dark blur
- All optionally take a `<span class="dot">` for a leading status dot

---

## Interactions & Motion

### Scroll-driven (key patterns to preserve)

#### Pinned horizontal scroll (used in `#showcase` and `#why`)
The CSS pattern: a tall `.runway` with `height: NNvh` contains a sticky `.pin` at `top:0; height:100vh`. JS reads `runway.getBoundingClientRect().top / (runway.offsetHeight - vh)` as a 0-1 progress value and applies `translateX(-progress * (track.scrollWidth - viewport.clientWidth))` to the horizontal track.

- **`#showcase`**: 380vh runway, 3 cards full-viewport-wide, **inertia easing** via `requestAnimationFrame` lerp (`current += (target - current) * 0.12`), per-image **parallax** based on each image's distance from viewport center
- **`#why`**: 340vh runway, 6 cards 380px wide, last card stops mid-viewport via `padding-right: calc(50vw - 200px)`, animated SVG wave background fills with `stroke-dashoffset` driven by scroll progress

**Mobile / touch fallback**: at `(max-width:900px) or (hover:none)`, runway collapses to natural height and the viewport becomes a horizontal swipe with `scroll-snap-type: x mandatory`. JS exits early on these viewports.

#### Footer reveal
The page is wrapped in `<div class="page-content">` with white-card background and 32px bottom corners; the `<footer class="site-footer">` is `position: fixed; bottom: 0; height: 740px; z-index: 1`. `body` has `padding-bottom: 740px` to give scroll-room. The page literally scrolls **over** the fixed footer, revealing it through the rounded bottom corners.

#### Sticky scrollspy TOC
`#seo-text` sidebar uses `IntersectionObserver` with `rootMargin: '-30% 0px -60% 0px'` to highlight the current heading.

### Hover & focus

- **Property cards** (`.prop`): translate `Y -4px`, image scale `1.04` over 800ms cubic-bezier
- **SEO tiles** (`.seo-card`): card translates `-3px`, bg image scales `1.06`
- **Blog cards** (`.article-card`): card lifts, image scales `1.08` + brightens down, hidden CTA "Читать статью" slides up from bottom, gold underline draws under the title left-to-right (scaleX origin left)
- **Showcase CTA**: pill background swaps from white-translucent to `--gold`; inner arrow chip swaps from gold-on-ink to ink-on-gold
- **Article underline**: `2px` gold bar under the title, `scaleX(0)` → `scaleX(1)` on hover, 550ms cubic-bezier

### Modal
Lead form modal (`#leadModal`) opens via any element with `data-open-modal="lead"`. Backdrop is blurred (`backdrop-filter: blur(8px)`); card animates with `popIn` keyframe (translateY 16px + scale 0.98 → identity). `Escape` key closes.

### Reveal on scroll
Any element with `data-reveal` starts `opacity:0; transform:translateY(28px)` and gains `.is-in` (→ identity) via a single `IntersectionObserver` (threshold 0.12, once-only). 900ms easing.

### Marquee
`.ticker__track` animates `translateX(0) → translateX(-50%)` over 40s linear infinite. Content is duplicated so it loops seamlessly.

---

## State Management

The prototypes are fully self-contained vanilla JS. Map this state into your framework's idioms (React `useState`/`useReducer`, Pinia store, Svelte store, etc.):

| State | Where | Trigger |
|---|---|---|
| Sticky header pinned | `#topbar.is-pinned` | `window.scrollY > 24` |
| Advanced search open | `#searchExtended.is-open` + `#searchAdvancedToggle.is-open` | Click on advanced toggle |
| Modal open | `#leadModal.is-open` + `body.overflow: hidden` | `data-open-modal` click |
| Why us scroll progress | `--p` (% width) on bar, `--p-num` (0-1) on wave SVG | Scroll position within runway |
| Why us active card # | text content of `#whyCur` | Same |
| Showcase active card # | text content of `#scCur` | Same |
| Showcase track position | `translate3d(-currentX,0,0)` on track | RAF lerp toward `targetX` derived from scroll progress |
| Showcase parallax | per-image `translate3d(-offset,0,0)` | Each image's distance from viewport center × per-image `data-px` factor |
| Video category filter | `display:none` on `.vc[data-cat]` not matching | Tab click |
| Testimonials slider index | `translateX(-idx*step)` on `.tm__track` | Prev/next/dot click + window resize |
| Testimonial expand | `.tm-card.is-expanded` removes `-webkit-line-clamp` | "Читать полностью" click |
| Testimonial "more" button visibility | `.tm__more.hide` when `scrollHeight ≤ clientHeight` | Post-layout `requestAnimationFrame` measure |
| FAQ open | `.faq-item.is-open` + `.faq-a { max-height: scrollHeight }` | Question click; opening one closes others |
| Form errors | `.is-error` on field wrapper, `.is-success`/`.is-sent` on form on submit | Submit handler |
| SEO TOC active | `.is-active` on TOC link | IntersectionObserver on H2/H3s |
| Subscribe success | `.sf-sub.is-sent` | Footer subscribe submit |

---

## Assets

- **Hero background**: `design/assets/hero.jpg` — provided by client (Alanya seafront with palm trees). Replace if needed.
- **Logo**: Currently rendered as a text fallback `<span class="brand-fallback"><span class="plus">+</span>Plus Investment</span>`. Replace with the client's SVG/PNG logo once provided.
- **Property images**: Unsplash URLs are used as placeholders in cards (`#popular`, `#showcase`, `#video`, `#testimonials`, `#blog`, `#seo`). Replace with real images from the client's CMS / object database.
- **Avatars**: Unsplash portraits as placeholders in `#testimonials`. Replace with real photos.
- **Icons**: All inline SVG, drawn 1.5–1.8px stroke. No icon font / no third-party icon library required.

---

## Responsive

Two breakpoints used throughout:

- **`max-width: 1080px`** — navigation collapses (`.nav { display: none }`, burger should replace it; *not implemented in the prototype*), property grid → 2 col, search row → 4 col, hero lead form stacks.
- **`max-width: 900px`** — most multi-column grids collapse to 1, sticky aside in FAQ/SEO becomes static, scroll-jacked sections fall back to horizontal swipe with scroll-snap, video shorts → 4-col then 2-col, testimonials → 2 per view.
- **`max-width: 640px`** — utility top-row hides, all cards full-width, all multi-input forms stack.

**Note**: A real mobile burger menu is not yet implemented in the prototypes. Build a proper drawer/sheet for nav under 1080px in the target codebase.

---

## Copywriting

All UI copy is **Russian** (`<html lang="ru">`). EN and TR are scoped per ТЗ — implement i18n in the target codebase (next-intl, vue-i18n, etc.) using the keys from the prototype as the RU base.

Full content spec (in Russian) is in `spec/tz.txt`.

---

## Files in this bundle

```
design_handoff_plus_investment/
├── README.md                ← you are here
├── skill.md                 ← project conventions for Claude Code
├── design/
│   ├── index.html           ← full landing page prototype (all 13 blocks)
│   ├── design-system.html   ← style guide / token reference
│   └── assets/
│       └── hero.jpg
└── spec/
    └── tz.txt               ← original technical spec (RU), extracted from .docx
```

---

## Recommended implementation order

1. **Tokens & global CSS** — port `:root` custom properties, font imports, base typography utilities (`.sec-eyebrow`, `.sec-title`, `.btn`, `.badge`).
2. **Layout primitives** — `.wrap` container, section padding system.
3. **Header & footer** — these are present on every page; build first.
4. **Hero** — static, low risk, validates type/spacing/CTA loop.
5. **Search + Popular** — the page's primary conversion path.
6. **Modal + Lead form** — reuse the form patterns across the rest.
7. **Pinned horizontal scroll sections** (`#showcase`, `#why`) — most complex JS; isolate behind a reusable `<PinnedHorizontalScroll>` component.
8. **Testimonials slider, FAQ, Video tabs** — straightforward stateful components.
9. **SEO text** — typography showcase, blog-CMS shaped.
10. **Footer reveal effect** — single global wrapper; verify on all pages.

Stick to the system. The aesthetic depends on **restraint** (thin/light headings, lots of negative space, one gold accent at a time) — adding extra decoration breaks the editorial feel.
