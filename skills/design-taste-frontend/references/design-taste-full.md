# Design Taste Comprehensive Reference

Complete guidelines for anti-slop frontend engineering, official design systems, layout patterns, and AI-tell avoidance.

## 1. Design System Map

When the brief aligns with an established ecosystem, use official packages:

| Requirement | Package | Notes |
| --- | --- | --- |
| Microsoft / Enterprise | `@fluentui/react-components` | Official Fluent UI 9 |
| Material Design 3 | `@material/web` | Official Material Web Components |
| IBM Enterprise Analytics | `@carbon/react`, `@carbon/styles` | Carbon Design System |
| Shopify App UI | `polaris.js` web components | Required for Shopify admin |
| Atlassian / Jira-style | `@atlaskit/*` + `@atlaskit/tokens` | Official Atlassian DS |
| GitHub Devtool | `@primer/css`, `@primer/react-brand` | Official Primer; Brand for marketing |
| Accessible React Primitives | `@radix-ui/themes` | Polished accessible primitives |
| Owned Customizable UI | `shadcn/ui` (`npx shadcn@latest add ...`) | Customize tokens; never ship default state |
| Public Sector UK / US | `govuk-frontend` / `uswds` | Regulatory compliance standard |
| Fast Local Business MVP | Bootstrap 5.3 | Boring, fast, works |
| Modern SaaS / AI Marketing | Tailwind v4 utilities + `dark:` variant | Default for indie + small teams |

**Rules:**
- One design system per project. No mixing Fluent with Carbon or shadcn with Material.
- Install the **official** package. Do not recreate its CSS by hand or override 90% of its tokens.

---

## 2. Typography Discipline

- **Sans alternatives to Inter:** `Geist`, `Satoshi`, `Outfit`, `Cabinet Grotesk`, `PP Neue Montreal`.
- **Serif is very discouraged as default.** Acceptable only when the brand brief literally names a serif, or the aesthetic is genuinely editorial/luxury/publication. Never default to `Fraunces` or `Instrument_Serif`.
- **Emphasis rule:** Use italic or bold of the **same font family**. Never inject a random serif word into a sans headline for visual interest.
- **Italic descender clearance:** When italic is used in display type with descender letters (`y g j p q`), use `leading-[1.1]` minimum and add `pb-1` reserve. `leading-none` clips descenders.
- **Font loading:** Always use `next/font` or self-hosted `@font-face` with `font-display: swap`. Never link Google Fonts via `<link>` in production.

---

## 3. Color & Palette Rules

- Max 1 accent color. Saturation < 80% by default on neutral bases (Zinc/Slate/Stone).
- **Color Consistency Lock:** Once an accent color is chosen, it is used across the entire page. No color shifts between sections.
- **Premium-Consumer Palette Ban:** The following hex families are banned as default for premium-consumer briefs (cookware, wellness, artisan, luxury):
  - Backgrounds: `#f5f1ea`, `#f7f5f1`, `#fbf8f1`, `#efeae0` (warm paper/cream/chalk)
  - Accents: `#b08947`, `#b6553a`, `#9a2436` (brass/clay/oxblood)
  - Text: `#1a1714` (espresso/warm near-black)
- **Default alternatives:** Cold Luxury (silver-grey + chrome), Forest (deep green + bone), Cobalt + Cream, Terracotta + Slate, pure monochrome + single saturated pop.
- **No pure `#000000` or `#ffffff`.** Use off-black (zinc-950) and off-white.

---

## 4. Layout Hard Rules

- **Hero viewport fit:** Headline ≤ 2 lines, subtext ≤ 20 words AND ≤ 4 lines, CTAs visible without scroll. Top padding max `pt-24`.
- **Hero stack discipline:** Max 4 text elements (eyebrow OR brand strip, headline, subtext, CTAs). No tiny taglines below CTAs, no trust micro-strips in hero.
- **Eyebrow restraint:** Max 1 uppercase-tracking eyebrow per 3 sections. Hero counts as 1.
- **No 3-column identical cards.** Use asymmetric grid, bento, horizontal-scroll, or split-screen.
- **Zigzag alternation cap:** Max 2 consecutive image+text-split sections. The 3rd consecutive zigzag is a pre-flight fail. Break with a full-width, bento, or vertical-stack section.
- **Split-header ban:** No "left big headline + right small explainer paragraph" as section header. Stack vertically instead.
- **Bento cell count:** Exactly as many cells as content items. No empty filler cells.
- **Bento background diversity:** At least 2–3 cells need real visual variation (images, gradients, patterns), not all white-on-white text cards.
- **Section-layout-repetition ban:** No two sections share the same layout family. 8 sections need at least 4 different layout families.
- **Single-line navigation:** Desktop nav items must fit on one line, height ≤ 80px.
- **`min-h-[100dvh]` over `h-screen`:** Prevents layout jumping on mobile (iOS Safari address bar).

---

## 5. Canonical Motion Skeletons

### Sticky Stack (GSAP ScrollTrigger)
```tsx
"use client";
import { useRef, useEffect } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useReducedMotion } from "motion/react";

gsap.registerPlugin(ScrollTrigger);

export function StickyStack({ cards }: { cards: React.ReactNode[] }) {
  const ref = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();

  useEffect(() => {
    if (reduce || !ref.current) return;
    const ctx = gsap.context(() => {
      const cardEls = gsap.utils.toArray<HTMLElement>(".stack-card");
      cardEls.forEach((card, i) => {
        if (i === cardEls.length - 1) return;
        ScrollTrigger.create({
          trigger: card, start: "top top",
          endTrigger: cardEls[cardEls.length - 1], end: "top top",
          pin: true, pinSpacing: false,
        });
        gsap.to(card, {
          scale: 0.92, opacity: 0.55, ease: "none",
          scrollTrigger: { trigger: cardEls[i + 1], start: "top bottom", end: "top top", scrub: true },
        });
      });
    }, ref);
    return () => ctx.revert();
  }, [reduce]);

  return (
    <div ref={ref} className="relative">
      {cards.map((card, i) => (
        <div key={i} className="stack-card sticky top-0 min-h-[100dvh] flex items-center justify-center">{card}</div>
      ))}
    </div>
  );
}
```

### Horizontal Pan (GSAP ScrollTrigger)
```tsx
"use client";
import { useRef, useEffect } from "react";
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { useReducedMotion } from "motion/react";

gsap.registerPlugin(ScrollTrigger);

export function HorizontalPan({ children }: { children: React.ReactNode }) {
  const wrap = useRef<HTMLDivElement>(null);
  const track = useRef<HTMLDivElement>(null);
  const reduce = useReducedMotion();

  useEffect(() => {
    if (reduce || !wrap.current || !track.current) return;
    const ctx = gsap.context(() => {
      const distance = track.current!.scrollWidth - window.innerWidth;
      gsap.to(track.current, {
        x: -distance, ease: "none",
        scrollTrigger: {
          trigger: wrap.current, start: "top top",
          end: () => `+=${distance}`, pin: true, scrub: 1, invalidateOnRefresh: true,
        },
      });
    }, wrap);
    return () => ctx.revert();
  }, [reduce]);

  return (
    <section ref={wrap} className="relative overflow-hidden">
      <div ref={track} className="flex h-[100dvh] items-center">{children}</div>
    </section>
  );
}
```

### Scroll-Reveal Stagger (Motion)
```tsx
"use client";
import { motion, useReducedMotion } from "motion/react";

export function RevealStagger({ items }: { items: string[] }) {
  const reduce = useReducedMotion();
  return (
    <ul className="grid gap-6">
      {items.map((item, i) => (
        <motion.li
          key={item}
          initial={reduce ? false : { opacity: 0, y: 24 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true, amount: 0.3 }}
          transition={{ duration: 0.5, delay: i * 0.05, ease: [0.16, 1, 0.3, 1] }}
        >{item}</motion.li>
      ))}
    </ul>
  );
}
```

---

## 6. Comprehensive AI-Tell Ban List

1. **Zero em-dashes (`—`):** Banned across all visible copy. Use hyphens, periods, or commas.
2. **No neon/AI-purple glows.** Use clean neutrals with intentional contrast accents.
3. **No 3-column identical cards.** Use asymmetric grids, split screens, or bento layouts.
4. **No div-based fake screenshots.** Use real images, generated images, or explicit placeholder slots.
5. **No Jane Doe/Acme data.** Use realistic, contextual names and organic numbers.
6. **No section-number eyebrows.** Avoid `001 / Capabilities`, `02 · The Hardware`.
7. **No floating corner paragraphs.** Stack headline and body cleanly.
8. **No marquee overuse.** Maximum 1 horizontal text marquee per page.
9. **No scroll cues.** No "Scroll to explore", no animated mouse wheels.
10. **No mid-page theme inversion.** Uniform page theme (all light or all dark).
11. **No version labels in hero** (`V0.6`, `BETA`, `INVITE-ONLY`) unless the brief is a launch.
12. **No `window.addEventListener('scroll')`.** Use Motion's `useScroll()`, GSAP ScrollTrigger, IntersectionObserver, or CSS scroll-driven animations.
13. **No pills/labels overlaid on images.** No `Plate · Brand`, no `Field notes - journal`.
14. **No decorative status dots** (colored dots before nav items, list rows, etc.) unless conveying real semantic state.

---

## 7. Content Density & Copy

- **Section content shape:** Short headline (≤ 8 words) + short subparagraph (≤ 25 words) + one visual or CTA.
- **Long lists (> 5 items):** Use card grid, tabs/accordion, horizontal scroll-snap, or carousel instead of default `<ul>` with `divide-y`.
- **Quotes:** Max 3 lines of body. Attribution: name + role (+ company optional). No em-dash attribution.
- **Fake-precise numbers:** Numbers like `92%` or `4.1×` must come from real data or be explicitly labeled as mock. Never invent engineering precision the brand doesn't claim.
- **Copy self-audit:** Before shipping, re-read every visible string. Flag grammatically broken, AI-hallucinated, or unclear-referent phrases. Replace with plain functional text.

---

## 8. Redesign Protocol

### Mode Detection
- **Preserve:** Maintain brand tokens, URL slugs, and IA. Modernize typography, spacing, and micro-interactions.
- **Overhaul:** New visual language, preserve content and SEO metadata.

### Audit Before Touching
Document: brand tokens (colors, type, radii), IA and nav, content blocks (working vs filler), patterns to preserve vs retire, SEO baseline (ranking pages, meta, OG cards).

### Preservation Rules
- Never alter route slugs, nav labels, or form field names without explicit request.
- Extract brand colors before applying palette rules. A brand already using purple stays purple.
- Honor existing accessibility wins (focus states, alt text, keyboard nav, contrast).

### Modernization Levers (priority order)
1. Typography refresh (biggest visual lift per unit of risk)
2. Spacing & rhythm
3. Color recalibration
4. Motion layer
5. Hero & key-section recomposition
6. Full block replacement (only when unsalvageable)

---

## 9. Apple Liquid Glass Web Approximation

Not an official Apple web package. This is a `backdrop-filter` approximation labeled as such:

```css
.liquid-glass-approx {
  position: relative;
  isolation: isolate;
  overflow: hidden;
  border-radius: 999px;
  border: 1px solid rgb(255 255 255 / 0.32);
  background:
    linear-gradient(135deg, rgb(255 255 255 / 0.30), rgb(255 255 255 / 0.08)),
    rgb(255 255 255 / 0.12);
  backdrop-filter: blur(24px) saturate(180%) contrast(1.05);
  -webkit-backdrop-filter: blur(24px) saturate(180%) contrast(1.05);
  box-shadow:
    inset 0 1px 0 rgb(255 255 255 / 0.48),
    inset 0 -1px 0 rgb(255 255 255 / 0.12),
    0 18px 60px rgb(0 0 0 / 0.18);
}

@media (prefers-reduced-transparency: reduce) {
  .liquid-glass-approx {
    background: rgb(255 255 255 / 0.96);
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}
```
