# Phase 2 — Data & Badges Implementation Plan
> **Design:** [2026-04-12-phase2-badges-design.md](./2026-04-12-phase2-badges-design.md)
> **Date:** 2026-04-12
> **Status:** Ready to Execute

---

## Wave 1 — The Badge Engine

### Task 2.1 — Create `_includes/badge_gen.html`
```
<files>  _includes/badge_gen.html
<action> Reusable Liquid include. Accepts: include.label, include.color, include.logo.
         Constructs Shields.io for-the-badge URL. Apply url_encode to label.
         Output: <img> with loading="lazy", height="28", descriptive alt text.
         This is the ONLY place in the entire codebase that builds a Shields.io URL.
<verify> bundle exec jekyll build
<done>   Build passes. File exists at _includes/badge_gen.html.
```

---

## Wave 2 — Wire Badges Into Sections

### Task 2.2 — Update `_includes/sections/projects.html`
```
<files>  _includes/sections/projects.html
<action> Replace the plain <span class="stack-tag"> loop with badge_gen.html calls.
         Use forloop.index0 to index into parallel arrays: stack_colors[], stack_logos[].
         Pattern:
           {% for tech in project.stack %}
             {% assign idx = forloop.index0 %}
             {% assign color = project.stack_colors[idx] %}
             {% assign logo  = project.stack_logos[idx]  %}
             {% include badge_gen.html label=tech color=color logo=logo %}
           {% endfor %}
         Keep limit:6 on the outer project loop — unchanged.
<verify> bundle exec jekyll build && Select-String -Path "_site/index.html" -Pattern "img.shields.io" | Measure-Object | Select-Object Count
<done>   Build passes. img.shields.io URLs appear in _site/index.html (at least 3 — one per stack item).
```

### Task 2.3 — Update `_includes/sections/contact.html`
```
<files>  _includes/sections/contact.html
<action> Replace plain text links with Shields.io badge images wrapped in anchors.
         Each social item: <a href=url><img from badge_gen></a>
         Pass item.platform as label, item.color as color, item.logo as logo.
         Remove the Phase 1 placeholder paragraph.
<verify> bundle exec jekyll build && Select-String -Path "_site/index.html" -Pattern "contact__badge" | Measure-Object
<done>   Build passes. Contact section renders badge <img> tags linked to social URLs.
```

---

## Wave 3 — Documentation

### Task 2.4 — Create `_data/profile.example.json`
```
<files>  _data/profile.example.json
<action> Fully documented JSONC reference file. Every single field must have an inline
         comment explaining: what it does, what values are valid, and any warnings.
         Key warnings to include:
         - stack/stack_colors/stack_logos MUST be same length (parallel arrays)
         - social[].color must be hex WITHOUT # prefix
         - theme_config.vanta_effect must be one of 6 valid strings
         - projects array: max 6 items — extras silently ignored
         - profile_image_url must be HTTPS external URL (no local files)
<verify> ruby -e "require 'json'; JSON.parse(File.read('_data/profile.example.json').gsub(/\/\/.*/, '')); puts 'Example JSON parses'"
<done>   File exists. All schema fields present and commented.
```

---

## Final Verification
```powershell
bundle exec jekyll build
# Expect: done in <2 seconds, zero errors

Select-String -Path "_site/index.html" -Pattern "img.shields.io" | Measure-Object
# Expect: Count >= 6 (3 stack badges × 2 projects + 3 social badges)
```
