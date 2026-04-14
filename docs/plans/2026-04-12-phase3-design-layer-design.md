# Phase 3 — Design Layer & Visual Engine Design
> **Approach:** Step-wise refinement (Vanta.js + UI/UX Pro Polish)
> **Date:** 2026-04-12
> **Status:** Approved

---

## Goal
Fully implement the dynamic Vanta.js animated background supporting all 6 effects defined in the JSON. Elevate the base SCSS styles implemented in Phase 1 to "premium" UI/UX standards (smooth micro-interactions, precise glassmorphism, responsive polish).

---

## What Changes

| Area | Prior State (Phase 1/2) | Target State (Phase 3) |
| :--- | :--- | :--- |
| `_includes/vanta_init.html` | Empty placeholder stub | Full logic: dynamic CDN loading based on `vanta_effect`, JS init script with `try/catch` |
| `assets/css/main.scss` | Solid structural styling | Enhanced hover states, polished animations, adjusted z-indices to support Vanta canvas |
| Hero Section | Static gradient background | Interactive Vanta canvas injected exactly at `#hero` with fallback protection |

---

## The Vanta.js Architecture

1. **Gatekeeping**: `default.html` already checks `{% if site.data.profile.theme_config.vanta_effect != "" %}` before including the component.
2. **Dynamic CDN**: The included script constructs the Vanta URL using `{{ vanta | downcase }}` so we only load the exact animation requested (cloud, net, waves, etc.).
3. **Execution Environment**:
   - Requires Three.js as a prerequisite (also loaded via CDN).
   - Injected into `#hero` container.
   - Tied to JSON colors (`--color-accent` and `--color-primary`).
   - Uses `try/catch` to gracefully fail and preserve readability if ad-blockers block the CDN.

---

## Design Refinements (UI/UX Pro Integration)

- Ensure `.hero__content` has a relative position and z-index > 0 so text appears *above* the Vanta canvas.
- Ensure `#hero` background is set to `transparent` so Vanta can be seen.
- Refine existing glassmorphism: verify `--color-secondary-rgb` style implementations or similar to ensure the nav blurring works impeccably in both light and dark modes.

---

## Exit Criteria

- [ ] `bundle exec jekyll serve` completes without errors
- [ ] Vanta.js initializes successfully natively in the browser on page load
- [ ] Changing `vanta_effect` to `waves` or `clouds` in JSON properly changes the injected JS file
- [ ] Hero text remains legible above the animation
- [ ] Design maintains premium aesthetic in both Dark and Light modes

---

> **Next**: Execute `/plan` to generate atomic execution tasks.
