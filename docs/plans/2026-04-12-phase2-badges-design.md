# Phase 2 — Data & Badges Design
> **Approach:** Schema-First (consistent with Phase 1)
> **Date:** 2026-04-12
> **Status:** Approved

---

## Goal
Replace all Phase 1 placeholder text with real Shields.io badge images. Deliver a `profile.example.json` that fully documents every field so any developer can fork and fill it in confidently.

---

## What Changes

| Layer | Phase 1 | Phase 2 |
| :--- | :--- | :--- |
| Projects stack | `<span class="stack-tag">` plain text | Shields.io badge images via `badge_gen.html` |
| Contact social links | Plain text anchor | Shields.io badge image wrapped in anchor |
| `badge_gen.html` | Did not exist | New reusable Liquid include |
| `profile.example.json` | Did not exist | Fully documented JSONC reference file |

---

## Architecture

```
badge_gen.html (new)
  ↑ called by
projects.html  (updated — parallel array loop)
contact.html   (updated — social badge loop)
```

`badge_gen.html` is the single URL factory. No other file constructs Shields.io URLs inline.

---

## `badge_gen.html` Contract

**Inputs (via include parameters):**
- `include.label` — badge left-side text (e.g., "React")
- `include.color` — hex color WITHOUT `#` (e.g., "61DAFB") — Shields.io format
- `include.logo` — Simple Icons logo slug (e.g., "react")

**Output:**
```html
<img src="https://img.shields.io/badge/{label}-{color}?style=for-the-badge&logo={logo}&logoColor=white"
     alt="{label} badge" loading="lazy" height="28" />
```

**Edge cases handled:**
- Label with spaces → `url_encode` filter applied (e.g., "Node.js" → "Node.js")
- Missing logo → renders badge without logo (graceful, not broken)

---

## Parallel Array Safety

The `stack`, `stack_colors`, `stack_logos` arrays must be equal length. The include uses `forloop.index0` to index into the parallel arrays. Documented clearly in `profile.example.json` with a warning.

---

## Exit Criteria

- [ ] `bundle exec jekyll build` still passes with zero errors
- [ ] Badge images appear in Projects section (one per stack item)
- [ ] Badge images appear in Contact section (one per social entry)
- [ ] `badge_gen.html` is the ONLY place Shields.io URLs are constructed
- [ ] `profile.example.json` documents every field in the schema

---

> **Next:** `/plan` → `/execute`
