---
name: design-taste-frontend
description: Anti-slop frontend engineering for landing pages, portfolios, and marketing surfaces. Enforces distinctive design direction, real design systems, and strict pre-flight checks.
---

# design-taste-frontend

Distinctive frontend engineering for landing pages, portfolios, and redesigns.

## 1. Brief Inference

Before writing code or components, state a one-line Design Read:
`Reading this as: <page kind> for <audience>, with a <vibe> language, leaning toward <foundation/aesthetic>.`

### Anti-Default Discipline
Avoid generic LLM defaults: AI-purple glow gradients, centered hero over dark mesh, 3 identical feature cards, Inter font, and generic glassmorphism everywhere.

---

## 2. The Three Dials

- **`DESIGN_VARIANCE` (1–10):** `1` = Symmetric grid, `10` = Asymmetric, masonry, offset whitespace.
- **`MOTION_INTENSITY` (1–10):** `1` = Static/CSS hover, `10` = Choreographed scroll & spring physics.
- **`VISUAL_DENSITY` (1–10):** `1` = Generous whitespace, `10` = High data density.
- **Default Baseline:** `8 / 6 / 4`.

---

## 3. Core Directives

### Typography
- Display: `text-4xl md:text-6xl tracking-tighter leading-none`.
- Sans alternatives to Inter: `Geist`, `Satoshi`, `Outfit`, `Cabinet Grotesk`.
- Serif only when explicitly requested or authentic to luxury/editorial brand. Never default to `Fraunces` or `Instrument_Serif`.

### Color & Palette
- Lock **one** accent color for the entire page. Saturation < 80% on neutral bases (Zinc, Slate, Stone).
- Avoid generic beige/brass/espresso combinations for consumer brands.

### Layout & Composition
- **Hero Viewport:** Headline ≤ 2 lines, subtext ≤ 20 words (≤ 4 lines), CTAs visible without scroll. Top padding max `pt-24`.
- **Eyebrow Restraint:** Maximum 1 uppercase tracking eyebrow per 3 sections.
- **No 3-Column Equal Cards:** Use asymmetric grids, split screens, bento grids, or horizontal lists.
- **Single Line Navigation:** Nav items must fit on one line at desktop (height ≤ 80px).

### Imagery & Real Assets
- Use real photography (`picsum.photos/seed/...` or generated assets). Never use div-based fake screenshots.
- Social proof: Use official SVG brand logos (Simple Icons / Devicon), not text wordmarks.

### Hard AI-Tell Bans
- **Zero em-dashes (`—`)** anywhere visible (headlines, body, buttons, captions, quotes). Use hyphens or rephrase.
- **No fake product UIs** built with styled divs.
- **No `window.addEventListener('scroll')`** — use Motion's `useScroll`, IntersectionObserver, or CSS scroll-driven animations.
- **No unreduced motion:** Wrap animations in `useReducedMotion()`.

---

## 4. Pre-Flight Checklist

Before delivering frontend work, verify:

- [ ] One-line Design Read declared.
- [ ] Zero em-dashes (`—`) in any visible copy.
- [ ] Page theme locked (no mid-page dark/light flips).
- [ ] Single accent color consistent across all sections.
- [ ] Hero fits within initial desktop viewport (top padding ≤ `pt-24`).
- [ ] Eyebrow count ≤ `ceil(sectionCount / 3)`.
- [ ] No 3 identical feature cards; bento has varied backgrounds and exact cell count.
- [ ] Buttons and form fields meet WCAG AA contrast (4.5:1).
- [ ] `prefers-reduced-motion` honored for all transitions.
- [ ] Motion components isolated in Client leaves with `'use client'`.

---

## Detailed References

For full design system packages, complete AI-tell catalogs, redesign protocols, and CSS skeletons:
- See [Design Taste Full Reference](./references/design-taste-full.md)
