# Emil Design Engineering Reference

In-depth implementation patterns and physics for fluid UI engineering.

## 1. Spring Animations

Springs simulate real physics and settle based on physical parameters without fixed durations.

### Apple-Style Spring (Recommended)
```javascript
{ type: "spring", duration: 0.5, bounce: 0.2 }
```

### Traditional Physics
```javascript
{ type: "spring", mass: 1, stiffness: 100, damping: 10 }
```

### Motion `useSpring` for Mouse Interactions
```jsx
import { useSpring } from 'motion/react';

// Interpolate continuous values with momentum
const springRotation = useSpring(mouseX * 0.1, { stiffness: 100, damping: 10 });
```

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

### Origin-Aware Popovers
```css
.popover {
  transform-origin: var(--transform-origin);
}
```

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

### Blur to Mask Imperfect Crossfades
Apply temporary `filter: blur(2px)` during crossfades between different component states to blend the transition smoothly.

---

## 3. CSS Transform & Clip-Path Mastery

- **Percentages in `translateY`:** `translateY(100%)` moves an element by its own height, regardless of pixel dimensions.
- **`clip-path: inset()` reveals:**
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

---

## 4. Gesture & Drag Interactions

- **Velocity-Based Dismissal:** Calculate `Math.abs(dragDistance) / elapsedTime`. If velocity > `0.11`, dismiss regardless of distance.
- **Boundary Damping:** Apply increasing friction when dragging past natural bounds instead of hitting a hard wall.
- **Pointer Capture:** Set pointer capture on drag start so gestures continue if the cursor leaves the element boundary.

---

## 5. The Sonner Principles

1. **Zero-Setup DX:** `<Toaster />` mounted once; `toast()` callable from anywhere without context hooks.
2. **Quality Defaults:** Excellent out-of-the-box easing, timing, and typography.
3. **Invisible Edge-Case Handling:** Pause timers when browser tab is hidden; pseudo-elements fill gaps between stacked toasts to maintain hover state.
4. **Asymmetric Timing:** Actions requiring deliberate choice are slow (e.g. hold-to-delete: 2s); interface responses are fast (release: 200ms).

---

## 6. Stagger Animations

Cascade entry of multiple elements with short delays (`30–80ms` between items):
```css
.item:nth-child(1) { animation-delay: 0ms; }
.item:nth-child(2) { animation-delay: 50ms; }
.item:nth-child(3) { animation-delay: 100ms; }
```

---

## 7. Debugging Checklist

- Play animations in DevTools at 25%–50% speed to check easing, origin, and color sync.
- Test touch interactions on physical devices or remote inspector.
- Verify that `prefers-reduced-motion` cleanly disables transform motion.
