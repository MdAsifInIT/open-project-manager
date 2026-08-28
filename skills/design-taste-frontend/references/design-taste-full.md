# Design Taste Comprehensive Reference

Complete guidelines for anti-slop frontend engineering, official design systems, layout patterns, and AI-tell avoidance.

## 1. Design System Map

When the brief aligns with an established ecosystem, use official packages rather than custom approximations:

| Requirement | Package | Notes |
| --- | --- | --- |
| Microsoft / Enterprise | `@fluentui/react-components` | Official Fluent UI 9 |
| Material Design 3 | `@material/web` | Official Material Web Components |
| IBM Enterprise Analytics | `@carbon/react`, `@carbon/styles` | Carbon Design System |
| Accessible React Primitives | `@radix-ui/themes` | Polished accessible primitives |
| Owned Customizable UI | `shadcn/ui` (`npx shadcn@latest add ...`) | Customize tokens; never ship default state |
| GitHub Devtool UI | `@primer/css`, `@primer/react-brand` | Official Primer system |
| Public Sector UK / US | `govuk-frontend` / `uswds` | Regulatory compliance standard |

*Rule:* One design system per project. Do not mix disparate component libraries in the same tree.

---

## 2. Canonical Motion Skeletons

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
          trigger: card,
          start: "top top",
          endTrigger: cardEls[cardEls.length - 1],
          end: "top top",
          pin: true,
          pinSpacing: false,
        });
        gsap.to(card, {
          scale: 0.92,
          opacity: 0.55,
          ease: "none",
          scrollTrigger: {
            trigger: cardEls[i + 1],
            start: "top bottom",
            end: "top top",
            scrub: true,
          },
        });
      });
    }, ref);
    return () => ctx.revert();
  }, [reduce]);

  return (
    <div ref={ref} className="relative">
      {cards.map((card, i) => (
        <div key={i} className="stack-card sticky top-0 min-h-[100dvh] flex items-center justify-center">
          {card}
        </div>
      ))}
    </div>
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
        >
          {item}
        </motion.li>
      ))}
    </ul>
  );
}
```

---

## 3. Comprehensive AI-Tell Ban List

The following patterns indicate unrefined AI generation and must be avoided:

1. **Zero Em-Dashes (`—`):** Banned across all user-facing copy. Use regular hyphens, periods, or commas.
2. **No Neon / AI-Purple Glows:** Use clean neutral backgrounds (Zinc, Slate) with intentional contrast accents.
3. **No 3-Column Identical Cards:** Avoid three identical cards horizontally for features.
4. **No Fake Screenshots in Divs:** Use real images or clear placeholder slots (`<!-- image -->`).
5. **No Jane Doe / Acme Data:** Use realistic, contextual names and numbers.
6. **No Section-Number Eyebrows:** Avoid `001 / Capabilities`, `02 · The Hardware`.
7. **No Floating Corner Paragraphs:** Stack headline and body cleanly or build balanced 2-column layouts.
8. **No Marquee Overuse:** Limit horizontal scrolling text strips to maximum 1 per page.
9. **No Scroll Cues:** Do not add animated mouse wheels or "Scroll to explore" text.
10. **No Mid-Page Inversion:** Maintain uniform page theme (all light or all dark).

---

## 4. Redesign Protocol

### Mode Detection
- **Preserve Mode:** Maintain brand tokens, URL slugs, and information architecture; modernize typography, spacing, and micro-interactions.
- **Overhaul Mode:** Complete visual reimagination while preserving content and SEO metadata.

### Preservation Rules
- Never alter route slugs, primary navigation labels, or form field IDs without explicit request.
- Audit existing brand colors and accessibility standards before replacing styles.

---

## 5. Appendices

### Apple Liquid Glass Web Approximation
```css
.liquid-glass-approx {
  position: relative;
  isolation: isolate;
  overflow: hidden;
  border-radius: 999px;
  border: 1px solid rgb(255 255 255 / 0.3);
  background: linear-gradient(135deg, rgb(255 255 255 / 0.25), rgb(255 255 255 / 0.05));
  backdrop-filter: blur(24px) saturate(180%);
  -webkit-backdrop-filter: blur(24px) saturate(180%);
  box-shadow: inset 0 1px 0 rgb(255 255 255 / 0.4), 0 18px 60px rgb(0 0 0 / 0.15);
}
@media (prefers-reduced-transparency: reduce) {
  .liquid-glass-approx {
    background: rgb(255 255 255 / 0.95);
    backdrop-filter: none;
  }
}
```
