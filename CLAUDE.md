# CLAUDE.md

Project guide for Claude Code. Read this before making changes.

## What this is

**Shotsmith** — a browser-based App Store screenshot generator. Users drop in raw simulator screenshots, add a localized title/subtitle, and export store-ready images at exact App Store dimensions. Localized for **English / Japanese / Korean** (both the output text and the UI).

Live: [https://gyugyu86.github.io/app-store-screenshot-studio/](https://gyugyu86.github.io/app-store-screenshot-studio/)

## Core principles (do not violate)

1. **Lightweight and local-first.** Single static page. **No backend, no build step, no runtime dependencies, no tracking, no accounts.** It must keep working by just opening `index.html` (served over http for fonts) or via GitHub Pages.  
2. **Stay focused — resist feature bloat.** The whole value proposition is "fast, simple, does the essential job." See docs/ROADMAP.md for the explicit *do-not-build* list. When in doubt, leave it out.  
3. **Exact output specs are non-negotiable.** App Store rejects wrong sizes and alpha channels. Never regress these (see "Output contract" below).

## Architecture

- `index.html` — the entire app: HTML \+ CSS \+ vanilla JS \+ HTML Canvas. No framework.  
- `fonts/` — bundled **Noto Sans JP** and **Noto Sans KR** (variable WOFF2, SIL OFL). Required for correct CJK rendering on all OSes; do not remove or replace with CDN/system fonts.  
- `README.md` (landing), `MANUAL.md` (user guide), `DEPLOY.md` (publish guide), `docs/ROADMAP.md` (plan), `LICENSE` (MIT), `preview.png`.

### Key implementation details (the tricky parts — preserve these)

- **No-alpha PNG export:** browser `canvas.toBlob('image/png')` always embeds an alpha channel, which App Store Connect rejects. We hand-encode a 24-bit RGB PNG (color type 2\) using `CompressionStream('deflate')` for the IDAT. If `CompressionStream` is unavailable, we fall back to JPEG. **Do not "simplify" this back to `toBlob` PNG.**  
- **CJK font-load gate:** canvas bakes whatever glyphs are loaded *at draw time*. We `await` an explicit `document.fonts.load(...)` for Noto JP/KR weights \+ `document.fonts.ready` before every render/export, or Japanese/Korean exports get fallback/tofu glyphs. **Keep this gate.**
- **Batch ZIP export (no dependency):** "Export all" packs every render into one `shotsmith_screenshots.zip` via a small hand-rolled ZIP writer (`makeZip`) using the **STORE** method (no compression) — PNG/JPEG are already compressed, so DEFLATE would buy ~0 size for a whole inlined library. It **reuses `crc32()`** from the PNG encoder, and each image still goes through `exportBlob()`, so the font gate and no-alpha encoder apply identically. ZIP entries are foldered `{locale}/{device}_{NN}.{ext}`; **iPhone/iPad + EN/日本語/한국어 include checkboxes** beside the button filter which device/locale files are packed (locale boxes auto-follow the authored languages via `langChecked()`, overridable — this replaced the old "EN always exported" rule). **Don't swap this for a CDN/JSZip script** — it would break the single-file principle.  
- **Per-locale data model:** each slide holds `loc:{en,ja,ko:{title,subtitle}}` \+ shared `img:{iphone,ipad}`. Screenshots are shared across locales; only text is translated. Empty JA/KR fields fall back to EN at render/export time.  
- **UI i18n:** `STRINGS{en,ja,ko}` \+ `t()` \+ `applyI18n()`. Static text uses `data-i18n` / `data-i18n-ph` attributes; dynamic strings (toasts, slide chips, badges) call `t()`. The language tab drives both the authored content and the interface language.  
- **Layout:** top-text layout. `Fit` keeps margins; `Fill` enlarges the screenshot below the text (aspect ratio preserved, headline never covered). `Trim top` crops the screenshot's top (source rect) to remove the simulator status bar; rounded corners reapply to the new top edge.

## Output contract (must not regress)

- iPhone 6.5″ \= **1284×2778**, iPad 13″ \= **2064×2752**, exact pixels. *(Changed from 6.9″ 1320×2868: App Store Connect's iPhone screenshot slot is the 6.5″ display, which accepts 1242×2688 or 1284×2778 — we output the higher-res 1284×2778. If a listing needs 6.9″ again, add a size toggle rather than swapping.)*  
- PNG output \= **RGB, no alpha**. JPEG is an accepted alternative.  
- Export naming — single ("Download current"): `{locale}_{device}_{NN}.{ext}` (e.g. `ja_ipad_03.png`). Batch ("Export all"): one `shotsmith_screenshots.zip` whose entries are foldered `{locale}/{device}_{NN}.{ext}` (e.g. `ja/iphone_01.png`) — one folder per locale, drop straight into App Store Connect's per-locale slots.  
- 1–10 slides per device class.

## Working rules

- Keep everything in `index.html` unless there's a strong reason to split (splitting loses the single-file simplicity — discuss before doing it).  
- After any change, sanity-check: JS has no syntax errors, all referenced element IDs exist, all `data-i18n` keys exist in **all three** locales, and the no-alpha/font-gate logic is intact.  
- Don't add external network calls, analytics, or third-party scripts.  
- Update `MANUAL.md` when user-facing behavior changes; update `docs/ROADMAP.md` when scope changes.  
- Specs drift yearly (Apple adds devices). If updating sizes, keep the current ones as the documented primaries and note the change.

## Context

This project was designed and prototyped in a separate planning conversation. The feature-demand analysis, localization approach, and the essential-vs-bloat decisions live in **docs/ROADMAP.md** — treat it as the source of truth for *what to build next and what to avoid*.  
