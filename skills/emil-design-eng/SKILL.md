---
name: emil-design-eng
description: UI polish, component mechanics, micro-interactions, and animation decisions based on Emil Kowalski's design engineering philosophy.
---

# emil-design-eng

Refined UI craft, interaction details, and animation physics.

## Core Rules

1. **Never animate keyboard-initiated actions.** Repeated actions (command palette, search toggle) must be instant.
2. **Never animate from `scale(0)`.** Start from `scale(0.95)` with `opacity: 0`.
3. **Make popovers origin-aware.** Set `transform-origin: var(--transform-origin)` matching trigger (modals remain centered).
4. **Snappy button feedback:** Add `transform: scale(0.97)` with `160ms ease-out` on `:active`.
5. **UI duration cap:** Keep interface animations under `300ms` (standard: `150-250ms`).
6. **No `ease-in` on UI elements:** Use `ease-out` (entering), `ease-in-out` (morphing), or custom cubic-beziers.

## Review Format

When reviewing UI code, use this markdown table:

| Before | After | Why |
| --- | --- | --- |
| `transition: all 300ms` | `transition: transform 200ms ease-out` | Specify exact properties; avoid `all` |
| `transform: scale(0)` | `transform: scale(0.95); opacity: 0` | Objects in reality do not emerge from zero scale |
| `ease-in` on dropdown | `cubic-bezier(0.23, 1, 0.32, 1)` | `ease-in` feels sluggish; strong ease-out gives instant feedback |
| No `:active` on button | `transform: scale(0.97)` on `:active` | Buttons must feel responsive to touch/click |

## Animation Decision Matrix

| Frequency | Decision | Typical Duration | Easing |
| --- | --- | --- | --- |
| 100+/day (shortcuts, command palette) | No animation | 0ms | None |
| Frequent (dropdowns, tooltips, selects) | Fast | 125–200ms | Strong ease-out (`cubic-bezier(0.23, 1, 0.32, 1)`) |
| Occasional (modals, drawers, toasts) | Standard | 200–350ms | Spring or custom curve |
| Rare/First-time (onboarding, delight) | Expressive | 350–600ms | Spring (`duration: 0.5, bounce: 0.2`) |

## Performance & Accessibility

- Animate **only** `transform` and `opacity` (GPU composited).
- Wrap animations in `@media (prefers-reduced-motion: reduce)` or `useReducedMotion()`.
- Use CSS transitions over keyframes for interruptible UI states.
- In Motion, use `transform: "translateX(100px)"` rather than `x: 100` for hardware acceleration under load.

---

## Detailed References

For implementation guides on spring physics, clip-path reveals, gestures, Sonner patterns, and debugging:
- See [Emil Deep Dive](./references/emil-deep-dive.md)
