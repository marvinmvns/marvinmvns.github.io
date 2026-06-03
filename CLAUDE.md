# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page personal/consulting landing site for **Marcus Segundo** (tech leadership, Data & AI, Open Finance, Cloud). The entire site is one self-contained file: `index.html` (~1950 lines) holding all HTML, CSS, and JavaScript inline. There is **no build step, no package manager, no dependencies to install**, and no tests.

Production lives at `https://marvinmvns.github.io` (GitHub Pages serving the repo root of `marvinmvns.github.io`). Any commit to the root of the default branch publishes within ~1 minute. `.nojekyll` disables Jekyll processing.

## Develop / preview

Open `index.html` directly in a browser, or serve the folder so relative asset paths and the Three.js `<script>` resolve:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

There is nothing to build, lint, or test — edits to `index.html` are the deliverable.

## Architecture (all inside `index.html`)

- **`<head>`** — SEO/OG/Twitter meta, JSON-LD `Person` schema, Google Fonts, and a single large `<style>` block. The design system is driven by CSS custom properties under `:root` (palette `--bg*`/`--txt*`/`--gold`/`--neon`, radii, `--maxw`). Reuse these variables rather than hardcoding colors.
- **`<body>`** — semantic sections anchored by `id` (`#servicos`, `#perfil`, `#abordagem`, `#formatos`, `#produtos`, `#trajetoria`, `#contato`), matching the nav links. Mobile nav is a separate `.mobile-menu` block duplicating the desktop links.
- **First `<script>` (~line 1464)** — vanilla-JS UI: particle-network canvas background, terminal typing animation, scroll progress bar + sticky header, burger menu, `IntersectionObserver` reveal animations + skill-bar fills, and the i18n engine.
- **Second `<script>` (~line 1838)** — loads Three.js (`r128` UMD global build) **from cdnjs**, then builds the hero 3D scene (icosahedron core, rings, starfield) with raw `THREE.*` calls. Honors a `compact` flag to reduce particle counts on weaker devices. Because Three.js comes from a CDN, the hero 3D scene needs internet to render; offline, only that scene disappears (it's a separate script block from the rest of the UI). The code relies on the global `THREE` object, so if you change versions keep a **non-module UMD** build, not an ES-module/importmap one.

## i18n — the key pattern to preserve

The site supports **PT / EN / ES** via a `data-i18n="key"` attribute on every translatable element. The mechanism (in the first script block):

1. **Portuguese is the source of truth, written inline in the markup.** On load, the code snapshots every `data-i18n` element's `innerHTML` into a `PT` object (`const PT={}` is filled at runtime, not authored).
2. `const I18N` holds only the **`en` and `es` override dictionaries**, keyed by the same `data-i18n` keys.
3. `applyLang(lang)` re-applies text: `pt` restores from the `PT` snapshot, `en`/`es` look up `I18N[lang][key]` and fall back to PT if a key is missing. It also sets `<html lang>` and toggles the active `#lang` button.

When adding or editing copy: write the Portuguese inline in the HTML, give the element a unique `data-i18n` key, and add the **same key** to both `en` and `es` in `I18N`. Keeping the inline PT text and the `data-i18n` keys in sync is essential — a key present inline but missing from `I18N` silently falls back to Portuguese.

## Asset layout

All media lives under `assets/`, organized by type. Keep new files in the matching subfolder and reference them with the full path:

```
assets/img/     gallery images (1.jpg…8.png), marcus-foto.jpg, doguia-cores.jpeg
assets/logos/   company + education logos (f1rst, itau, ibm, harlio, fastrackgps, systemplan, doguia, mackenzie, oswaldo-cruz, fiap, fgv)
assets/icons/   tech-stack icons (aws, arduino, azure, docker, git, java, kubernetes, linux, nodejs, postgresql, python, raspberrypi, spring, typescript)
assets/thumbs/  YouTube thumbnails (yt-*.jpg)
assets/video/   hero-robot.mp4
assets/docs/    cv-marcus-segundo.pdf
```

`index.html`, `README.md`, `CLAUDE.md`, and `.nojekyll` stay at the repo root. Three.js is **not** vendored here — it loads `r128` from cdnjs (see the script-blocks note above).

**Known missing file:** `assets/img/1.jpg` is referenced by the gallery but is not present in this checkout (returns 404). Add the real image or remove that gallery item.

Before assuming an asset is broken vs. missing: the deployed `marvinmvns.github.io` repo is expected to have the real `assets/` folder layout. If you change an image/script reference, keep it consistent with the `assets/...` convention the HTML already uses, and confirm whether the target file actually exists in the tree you're editing.
