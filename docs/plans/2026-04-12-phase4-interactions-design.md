# Phase 4 — Section Refinement & Interactions Design
> **Approach:** TDD-friendly Vanilla JS (No Frameworks)
> **Date:** 2026-04-12
> **Status:** Approved

---

## Goal
Finalize interactivity by adding smooth, native-feeling scroll navigation. Clicking navigation links should scroll seamlessly to the target section while correctly offsetting the scroll position so the section headers aren't hidden behind the fixed glassmorphism navigation bar.

---

## What Changes

| Area | Prior State (Phase 3) | Target State (Phase 4) |
| :--- | :--- | :--- |
| **JS Scripts** | None | Introduce `assets/js/smooth-scroll.js` and an include to load it |
| **Navigation** | Browser default jump-scroll | Smooth animated scroll with dynamic header offset |
| **`default.html`** | Missing script | Now injects the script right before `</body>` |

---

## Smooth Scroll Architecture

- **Vanilla JS**: No jQuery, no heavy libraries. We'll utilize `window.scrollTo()` with `behavior: 'smooth'`.
- **Dynamic Offset Calculation**: The fixed nav is height `var(--nav-height)`. We will dynamically query the `.nav` element height in JS, ensuring perfect alignment even if CSS media queries alter the navigation bar height on mobile.
- **Progressive Enhancement**: Ensure `href` jumps still work without JavaScript on basic browsers.

---

## Exit Criteria

- [ ] `assets/js/smooth-scroll.js` created.
- [ ] Included into `_layouts/default.html`.
- [ ] Build completes without errors.

---

> **Next**: Execute `/plan` to generate atomic execution tasks.
