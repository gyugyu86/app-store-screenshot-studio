# ROADMAP

Direction for Shotsmith, distilled from a feature-demand and feasibility study of existing App Store screenshot tools (web SaaS, open-source, and CLI). The guiding goal: **stay lightweight and do the essential job well.** This file is the source of truth for what to build next — and what to avoid.

## Guiding principle

Most paid/SaaS tools lose users to bloat and friction (long onboarding, 45-minute workflows, feature overload). Our edge is the opposite: fast, local-first, no account, does exactly what's needed to ship an App Store listing. Protect that.

## Status — shipped (v1 \+ current)

- Exact-dimension export: iPhone 1284×2778 (6.5″), iPad 2064×2752.  
- Alpha-free RGB PNG (CompressionStream encoder) \+ JPEG fallback.  
- Top-text layout: gradient/solid background, title/subtitle, color/size controls.  
- Multiple slides (up to 10), live preview.  
- **EN/JA/KR localization** of both content and UI, with bundled Noto Sans JP/KR and a font-load gate.  
- **Fit / Fill** screenshot sizing (aspect ratio preserved).  
- **Trim top** to remove the simulator status bar.  
- **Batch ZIP export** — "Export all" bundles every slide × device × locale into a single `shotsmith_screenshots.zip`, foldered `{locale}/{device}_{NN}.{ext}` (e.g. `ja/iphone_01.png`) for App Store Connect's per-locale slots.
- **Per-device export selection** — iPhone / iPad checkboxes on "Export all" so an iPhone-only (or iPad-only) listing doesn't produce the other class's files.

## Next up (v2) — high ROI, in priority order

1. **Project save / load.** Serialize state (text per locale, style, layout) to a JSON file the user can export and re-import to resume/version a deck. Images as base64 data URLs. *Difficulty: low–med.*  
2. **Optional device-frame toggle.** Default OFF. Draw a **generic** rounded bezel in canvas (no Apple trademarks/artwork — Apple's official bezels are under a marketing license and shouldn't be redistributed in an MIT repo) or accept a user-supplied frame PNG. *Difficulty: medium.*

## Decisions

- **2026-06-19 — Batch ZIP via hand-rolled STORE writer, not JSZip.** Shipped v2 #1. Exported PNG/JPEG are already compressed, so the archive stores entries uncompressed (STORE method); DEFLATE would yield ~0 size gain while forcing a ~90–100KB inlined library, breaking the "single file, no dependencies" principle. The small `makeZip()` reuses the PNG encoder's existing `crc32()`. Standard ZIP (no ZIP64) — adequate for realistic export sizes. Each image still flows through `exportBlob()`, so the font-load gate and the no-alpha RGB PNG encoder are preserved. Validated with `unzip -t` (CRC/integrity) and byte-equality round-trip.
- **2026-06-22 — iPhone export size 1320×2868 (6.9″) → 1284×2778 (6.5″).** Real App Store Connect rejection: the project's iPhone screenshot slot is the **6.5″ display**, which accepts 1242×2688 or 1284×2778, not the 6.9″ 1320×2868 we were emitting (iPad 2064×2752 was unaffected). Switched the single iPhone target to the higher-res **1284×2778**. Aspect (~19.5:9) is nearly identical, so the top-text layout is unchanged. Kept as a single size rather than a 6.9″/6.5″ toggle to stay minimal; revisit with a toggle only if a listing needs both slots.

## Later (v3) — only if users ask

- Google Play export: phone 1080×1920 (9:16) \+ feature graphic 1024×500.  
- Mac App Store sizes (1280×800 / 1440×900 / 2560×1600 / 2880×1800).  
- 6.7″ iPhone fallback set (1290×2796) toggle.  
- Logo/icon overlay; per-text font/size/color overrides.

## Do NOT build (bloat guardrails)

These are the exact features that made competitor tools slow and that our target users skip:

- 3D perspective mockups / rotation (heavy; rarely needed for compliance).  
- Large template libraries (1000+ templates, 10k asset/icon packs).  
- AI caption generation or AI auto-translation (adds API keys, cost, privacy concerns).  
- Cloud projects, accounts, team collaboration, brand kits.  
- A/B testing variants, direct App Store Connect upload integration.  
- Panoramic/connected canvas across slides; animated app-preview video export.  
- Apple Watch / Vision Pro / Apple TV frames; 150+ device coverage.

## Open questions / triggers to revisit

- If GitHub issues repeatedly request Google Play or real device frames, promote them.  
- If `CompressionStream` proves flaky across users' browsers, default photo-heavy slides to JPEG.  
- If bundled CJK fonts make load too heavy, lazy-load them as separate files (already separate in `/fonts`, loaded via `@font-face`).

## Reference — output specs (2026)

- iPhone 6.5″: 1284×2778 (App Store Connect's iPhone slot; 1242×2688 also accepted). *(6.9″ 1320×2868 was the earlier target — see Decisions.)*  
- iPad 13″: 2064×2752 (2048×2732 also accepted).  
- Format: PNG/JPEG, **no alpha**, sRGB, 1–10 per device class.  
- Verify in App Store Connect before each release; specs change as Apple adds devices.

