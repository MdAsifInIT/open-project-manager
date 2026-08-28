# Emil Design Engineering Reference

Implementation patterns and physics for fluid UI engineering.

## 1. Spring Animations

Springs simulate real physics — they settle based on physical parameters, not fixed durations.

### Apple-Style Spring (Recommended)
```javascript
{ type: "spring", duration: 0.5, bounce: 0.2 }
```

### Traditional Physics
```javascript
{ type: "spring", mass: 1, stiffness: 100, damping: 10 }
```

Keep bounce subtle (0.1–0.3). Avoid bounce in most functional UI. Reserve for drag-to-dismiss and playful interactions.

### Spring Interruptibility
Springs maintain velocity when interrupted — CSS animations and keyframes restart from zero. This makes springs ideal for gestures that the user might reverse mid-motion (expanding items, then pressing Escape).

### Motion `useSpring` for Mouse Interactions
```jsx
import { useSpring } from 'motion/react';

// Without spring: feels artificial, instant
const rotation = mouseX * 0.1;

// With spring: feels natural, has momentum
const springRotation = useSpring(mouseX * 0.1, { stiffness: 100, damping: 10 });
```

This pattern works only for **decorative** interactions (background tilt, parallax). For functional data (banking graphs, precision controls), use no animation.

---

## 2. Component Building Patterns

### Responsive Button Press
```css
.button {
  transition: transform 160ms ease-out;
}
.button:active {
  transform: scale(0.97);
}
```
Applies to any pressable element. Scale range: `0.95–0.98`.

### Origin-Aware Popovers
```css
.popover {
  transform-origin: var(--transform-origin);
}
```
Set `transform-origin` to the trigger's position. **Exception:** Modals keep `transform-origin: center` (not anchored to a trigger).

### Tooltip Delay Optimization
```css
.tooltip {
  transition: transform 125ms ease-out, opacity 125ms ease-out;
  transform-origin: var(--transform-origin);
}
/* Skip animation and delay on subsequent tooltips in a toolbar */
.tooltip[data-instant] {
  transition-duration: 0ms;
}
```

### Enter Transitions with `@starting-style`
```css
.toast {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 300ms ease, transform 300ms ease;

  @starting-style {
    opacity: 0;
    transform: translateY(100%);
  }
}
```
Replaces the common React pattern of `useEffect(() => setMounted(true), [])`. Use `@starting-style` when browser support allows; fall back to `data-mounted` otherwise.

### Blur to Mask Imperfect Crossfades
When a crossfade between two states looks off, add `filter: blur(2px)` during the transition. Without blur, two distinct objects overlap unnaturally. Blur bridges the visual gap. Keep under 20px — heavy blur is expensive, especially in Safari.

---

## 3. CSS Transform & Clip-Path Mastery

### Percentage Translations
`translateY(100%)` moves an element by its own height regardless of pixel dimensions. Prefer percentages over hardcoded pixel values.

### `scale()` Affects Children
Unlike `width`/`height`, `scale()` scales children proportionally. When scaling a button on press, text, icons, and content all scale together.

### 3D Transforms
`rotateX()`, `rotateY()` with `transform-style: preserve-3d` create real 3D effects (orbiting, coin flips, depth).
```css
.wrapper {
  transform-style: preserve-3d;
}
```

### Clip-Path Reveals
```css
/* Reveal from left to right */
.overlay {
  clip-path: inset(0 100% 0 0);
  transition: clip-path 200ms ease-out;
}
.button:active .overlay {
  clip-path: inset(0 0 0 0);
  transition: clip-path 2s linear;
}
```

Use cases: tab color transitions (duplicate tab list, clip active state), hold-to-delete, scroll-triggered image reveals, before/after comparison sliders.

---

## 4. Performance Rules

### CSS Variable Inheritance Trap
Changing a CSS variable on a parent recalculates styles for **all children**. In drawers/lists with many items, update `transform` directly on the element instead:
```javascript
// Bad: triggers recalc on all children
element.style.setProperty('--swipe-amount', `${distance}px`);

// Good: only affects this element
element.style.transform = `translateY(${distance}px)`;
```

### Web Animations API (WAAPI)
JavaScript control with CSS performance — hardware-accelerated, interruptible, no library needed:
```javascript
element.animate(
  [{ clipPath: 'inset(0 0 100% 0)' }, { clipPath: 'inset(0 0 0 0)' }],
  { duration: 1000, fill: 'forwards', easing: 'cubic-bezier(0.77, 0, 0.175, 1)' }
);
```

### CSS Animations vs JS Under Load
CSS animations run off the main thread. When the browser is busy loading content, Framer Motion animations (using `requestAnimationFrame`) drop frames while CSS animations remain smooth. Use CSS for predetermined animations; JS for dynamic, interruptible ones.

---

## 5. Gesture & Drag Interactions

- **Velocity-Based Dismissal:** Calculate `Math.abs(dragDistance) / elapsedTime`. If velocity > `0.11`, dismiss regardless of distance. A quick flick should be enough.
- **Boundary Damping:** Apply increasing friction past natural bounds instead of a hard wall. Things in real life slow down; they don't suddenly stop.
- **Pointer Capture:** Set pointer capture on drag start so gestures continue even if the cursor leaves the element.
- **Multi-Touch Protection:** Ignore additional touch points after drag begins to prevent position jumps from finger switches.

---

## 6. The Sonner Principles

1. **Zero-Setup DX:** `<Toaster />` mounted once; `toast()` callable from anywhere. No hooks, no context, no complex setup.
2. **Quality Defaults:** Ship beautiful out of the box. Most users never customize. Easing, timing, and visual design must be excellent by default.
3. **Invisible Edge-Case Handling:** Pause timers when browser tab is hidden. Fill gaps between stacked toasts with pseudo-elements to maintain hover state. Capture pointer events during drag.
4. **Asymmetric Timing:** Deliberate actions are slow (hold-to-delete: 2s linear). System responses are fast (release: 200ms ease-out).
5. **Cohesion:** Animation style matches the component's personality. A playful component can be bouncier. A professional dashboard should be crisp and fast.

---

## 7. Stagger Animations

Cascade entry of multiple elements with short delays (`30–80ms` between items):
```css
.item:nth-child(1) { animation-delay: 0ms; }
.item:nth-child(2) { animation-delay: 50ms; }
.item:nth-child(3) { animation-delay: 100ms; }
```
Keep stagger delays short. Long delays make the interface feel slow. Never block interaction while stagger plays.

---

## 8. Debugging Checklist

- Play animations at 25–50% speed in DevTools to check easing, origin, and color sync.
- Step through frame-by-frame in Chrome Animations panel to spot timing issues between coordinated properties.
- Test touch gestures on physical devices (connect via USB, visit local dev server by IP).
- Verify `prefers-reduced-motion` cleanly disables all transform-based motion while keeping opacity/color transitions.
- Review animations with fresh eyes the next day — imperfections invisible during development become obvious.
