# Phase 3 — Design Layer & Visual Engine Implementation Plan
> **Design:** [2026-04-12-phase3-design-layer-design.md](./2026-04-12-phase3-design-layer-design.md)
> **Date:** 2026-04-12
> **Status:** Ready to Execute

---

## Wave 1 — Vanta.js Integration

### Task 3.1 — Implement `_includes/vanta_init.html`
```
<files>  _includes/vanta_init.html
<action> Replace the current stub comment with the full Vanta.js initialization logic over Three.js.
         - Include `<script defer src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>`
         - Include `<script defer src="https://cdn.jsdelivr.net/npm/vanta@latest/dist/vanta.{{ vanta | downcase }}.min.js"></script>`
         - Note: `vanta` should be assigned from `site.data.profile.theme_config.vanta_effect` before calling the include, but we'll read it locally just to be robust. `{% assign vanta = site.data.profile.theme_config.vanta_effect %}`
         - Add `<script>` block with `document.addEventListener('DOMContentLoaded')`.
         - Inside, use `try { VANTA['{{ vanta | upcase }}']({ el: '#hero', color: '{{ site.data.profile.theme_config.colors.accent }}', backgroundColor: '{{ site.data.profile.theme_config.colors.primary }}' }); } catch(err) { console.error(err); }`.
         - The URL injection MUST use `downcase` filter. The init call MUST use `upcase` filter.
<verify> bundle exec jekyll build
<done>   Build passes. `vanta_init.html` has script tags for three.min.js and vanta.*.min.js.
```

---

## Wave 2 — CSS Polish for Hero & Vanta

### Task 3.2 — Update `assets/css/main.scss` for Vanta Z-indexing
```
<files>  assets/css/main.scss
<action> Adjust `.hero` and `.hero__content` so that the Vanta canvas does not obscure the text.
         - `.hero` itself: `background-color: transparent;` so Vanta's canvas shows (fallback applied on `body`).
         - `.hero__content`: `position: relative; z-index: 10;` so it sits above the Vanta `<canvas>`.
         - Verify glassmorphism in nav has correct fallbacks (rgb conversion is tricky in pure CSS vars unless defined as rgb, so we'll ensure we use standard `var(--color-secondary)` with `opacity` or hex fallback).
<verify> bundle exec jekyll build && grep -A 5 ".hero__content {" _site/assets/css/main.css
<done>   `z-index: 10` (or similar >0) is applied to `.hero__content`.
```

---

## Final Verification
```powershell
bundle exec jekyll build
# Expect: build succeeds without Liquid exceptions.
```
