# Plus Investment — Project Skill

> Drop this file at the root of the target repo (or as `.claude/skills/plus-investment.md`) so Claude Code loads it as project context. It describes the brand, design language and engineering conventions for the Plus Investment landing site.

---

## Project

**Plus Investment** — Russian-language premium real-estate landing for a Turkey-based agency. Target audience: investors, relocators, holiday buyers. Languages planned: RU (default), EN, TR.

The single landing page is composed of 13 sequential sections; see `README.md` in `design_handoff_plus_investment/` for the full map and per-section behaviour.

---

## Aesthetic direction (non-negotiable)

The design is **editorial / premium-tech minimalism**, not "real-estate slop". Hold this line — additional decoration, emoji, generic gradients or rounded-corner-with-left-border accent cards break the look.

- **Typography is the load-bearing visual.** Section titles use Inter at weight 300 (thin) with a single `<em>` sub-phrase at weight 500-600, plus optionally one gold-colored sub-phrase. Mixed weights inside a single heading is the signature move.
- **Color discipline.** Most surfaces are paper (`#eef0f2`) or ink (`#15171b`). **Gold (`#f5ca3a`) is an accent, not a fill** — used for the primary CTA, hover states, dots, one-word highlights. Never gradient backgrounds.
- **Mono captions everywhere.** JetBrains Mono uppercase with `.22em` letter-spacing for eyebrows, dates, IDs, specs, numbers. Always preceded by a 24×1px gold bar (`.sec-eyebrow::before`).
- **One accent per surface.** A card has either a gold badge OR a gold underline OR a gold dot — not all three.
- **Pill everything.** Buttons, chips, badges, icon buttons → all `border-radius: 999px`. Cards use `22px` (large) or `16px` (medium).
- **Negative space is the layout.** Sections use `clamp(80px, 8vw, 128px)` vertical padding. Don't compress.

---

## Design tokens

Port these to the target stack's token source (Tailwind config, CSS vars, design-tokens.json, etc.):

```css
:root {
  /* Colors */
  --ink:        #15171b;
  --ink-2:      #222429;
  --ink-3:      #2c2f36;
  --paper:      #eef0f2;
  --paper-2:    #e6e8eb;
  --paper-3:    #dadde1;
  --mute:       #9aa0a6;
  --text:       #1a1c20;
  --text-soft:  #54585f;
  --gold:       #f5ca3a;
  --gold-deep:  #e3b524;

  /* Radii */
  --radius-s: 10px;
  --radius-m: 16px;
  --radius-l: 22px;

  /* Layout */
  --container: 1320px;
  --pad:       clamp(20px, 4vw, 56px);

  /* Type */
  --sans: "Inter", ui-sans-serif, system-ui, -apple-system, "Helvetica Neue", Arial, sans-serif;
  --mono: "JetBrains Mono", ui-monospace, "SF Mono", Menlo, monospace;
}
```

Inter weights to load: 200, 300, 400, 500, 600, 700, 800. JetBrains Mono: 400, 500.

---

## Type scale

| Style | Spec |
|---|---|
| Hero H1 | Inter 300, `clamp(44px, 6.6vw, 104px)`, ls `-0.035em`, lh `.98`, max-width 18ch, balanced |
| Section H2 | Inter 300, `clamp(32px, 4vw, 54px)` to `clamp(36px, 4.6vw, 64px)`, ls `-0.025em`, lh `1.02` |
| Card H3 | Inter 500, 19-24px, ls `-0.015em`, lh 1.2 |
| Body L | Inter 300, 15-17px, lh 1.55-1.65, max-width 60ch |
| Body S | Inter 400, 13-14px, lh 1.55 |
| Eyebrow | JetBrains Mono 500, 11px, ls `.22em`, uppercase, `--text-soft` |
| Mono tag | JetBrains Mono 400-500, 10-12px, ls `.16-.22em`, uppercase |

**`<em>` inside a heading**: `font-style: normal; font-weight: 500-600` — and sometimes wrapped in a vertical gradient text fill: `background: linear-gradient(180deg, #fff, #a8acb3); -webkit-background-clip: text; color: transparent` on dark surfaces.

---

## Component conventions

### Buttons
Pill (`border-radius: 999px`) with an inline circular arrow chip that translates right on hover. Three weights:

- **Gold primary** (`.btn--gold`): hero CTA, "Подобрать недвижимость", "Подписаться"
- **Dark** (`.btn--dark` / `.search-submit`): secondary form submits
- **Ghost** (`.btn--ghost`): outline on dark surfaces

### Cards
Always:
- White or dark surface
- `1px solid rgba(20,22,26,.06)` (light) / `rgba(255,255,255,.08)` (dark)
- Radius `--radius-m` or `--radius-l`
- Hover: `translateY(-4px)` + `box-shadow: 0 30px 60px -30px rgba(15,17,22,.25)`

Never: drop shadow at rest, oversized rounded corners, colored borders.

### Forms
- **Search filter** & **footer subscribe**: stacked labels (mono caption above input)
- **Modal lead form**, **lead block**, **ask form**: floating labels (caption sits in input, shrinks/moves up on focus or filled)
- Error: red border `#ff6b6b`, label turns red, `.err` span below
- Success: replace form contents with green check + confirmation text

### Iconography
All icons are **inline SVG drawn at 1.5–1.8px stroke**. No icon font, no Lucide/Heroicons import. When adding new icons, match this stroke weight and line-cap style.

### Spacing rhythm
- Section padding: `clamp(80px, 8vw, 128px)` standard, `clamp(48px, 5vw, 80px)` tight
- Grid gaps: 14-24px between cards, 32-48px between major columns, 48-96px between section subgroups
- `gap` on flex/grid (never margin between siblings)

---

## Scroll & motion primitives

These three motion patterns are signature features of the design — **preserve them faithfully**.

### 1. Pinned horizontal scroll (Showcase + Why us)

Tall `.runway` wraps a sticky `.pin` that is `100vh` tall. JS converts vertical scroll progress within the runway into horizontal translation of an inner track. Mobile/touch fallback uses native scroll-snap.

Wrap this as a reusable component (e.g. `<PinnedHorizontalScroll runwayVh={380}>` in React) — both sections use it with different runway heights, card widths and decorative overlays.

The **Showcase** variant adds:
- **Inertia easing**: `current += (target - current) * 0.12` per RAF tick instead of binding 1-to-1
- **Per-image parallax**: each image translates `-= (imageCenter - viewportCenter) * parallaxFactor`

### 2. Footer reveal

Page content sits in a wrapper with `border-bottom-left-radius: 32px` and the footer is `position: fixed; bottom: 0` behind it. `body { padding-bottom: footerHeight }` creates the scroll-room. The page literally scrolls **over** the fixed footer through its rounded corners. No JS needed.

### 3. Reveal on scroll
Single `IntersectionObserver` (threshold `0.12`, once-only) toggles `.is-in` on every element with `data-reveal`. Element starts `opacity: 0; transform: translateY(28px)`, transitions over 900ms cubic-bezier.

---

## Code conventions

If the target stack is React/Next.js or similar:

- **TypeScript** with strict mode
- **Components** in PascalCase, kebab-case file names match the section blocks (`hero/`, `search-filter/`, `pinned-horizontal-scroll/`, etc.)
- **One section per route segment / one component per section**. Match the file structure to the section map in `README.md`.
- **Tailwind or CSS Modules** — either is fine; if Tailwind, define the design tokens in `tailwind.config.ts` under `theme.extend.colors` etc. and write semantic class names (`text-eyebrow`, `btn-gold`) as `@apply` recipes rather than inlining every utility.
- **Server components by default**; client components only for sections with scroll/motion JS (`pinned-horizontal-scroll`, `testimonials-slider`, `faq-accordion`, `header-sticky`, `modal`).
- **Form validation**: `react-hook-form` + `zod` recommended; mirror the existing regexes (`phone: /^[\+\d\s\-\(\)]{8,}$/`, `email: standard`).
- **i18n**: `next-intl` or equivalent. Russian is the source language. Don't hard-code copy in JSX — extract to message files from day one.
- **Images**: Next `<Image>` with explicit aspect-ratios; lazy-load everything below the fold. Hero image preloaded.
- **Analytics**: track CTA clicks (`Подобрать недвижимость`, `Заказать звонок`, `Получить подборку`, `Задать вопрос`, `Подписаться`) and form submissions.

---

## Don'ts (recurring temptations to resist)

- ❌ Don't add emoji anywhere — not in CTAs, not in cards, not in faux-decorative spots. The brand uses none.
- ❌ Don't replace inline SVG icons with an icon font/library — stroke weight and visual consistency will drift.
- ❌ Don't introduce additional brand colors — the palette is exactly ink + paper + gold. Need a status hue? Use `#5fbf72` (the green dot already in the header) for success, `#ff6b6b` for errors. Nothing else.
- ❌ Don't use any font besides Inter and JetBrains Mono.
- ❌ Don't bold a section title with `font-weight: 700`. The light/bold contrast in headings is achieved with `<em>` swapping the weight to 500-600. Going to 700 looks heavy and wrong.
- ❌ Don't add gradient backgrounds to cards. Gradients are only for: hero image overlay, footer ambient orbs, dark-block radial accents — and even then very low opacity.
- ❌ Don't compress vertical section padding to "look denser". The cinematic breathing room is the design.
- ❌ Don't replace the scroll-jacking with simple horizontal swipe on desktop. The pinned mechanic with inertia is a core feature, not a gimmick.

---

## Working with assets

- Real property/complex photos live in the client's CMS (TBD). The Unsplash URLs in the prototype are placeholders only — every `images.unsplash.com/...` reference is replaceable.
- Hero background is `design/assets/hero.jpg` (Alanya seafront). Client may provide a video later; the hero is structured to swap in a `<video>` or YouTube iframe in place of the `<img>`.
- Logo: SVG to be supplied by client. Until then, use the text fallback `<span class="brand-fallback"><span class="plus">+</span>Plus Investment</span>`.

---

## Reference files

When in doubt, open the prototypes:

- `design_handoff_plus_investment/design/index.html` — the source of truth for every section
- `design_handoff_plus_investment/design/design-system.html` — token visualisation page
- `design_handoff_plus_investment/spec/tz.txt` — the original Russian technical spec

Match what's there. Don't reinvent.
