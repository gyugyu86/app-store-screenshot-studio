# Shotsmith

A lightweight, local-first **App Store screenshot studio** that runs entirely in your browser.
Drop in your simulator screenshots, add a title and subtitle, and export **store-ready files at exact
App Store dimensions** — with built-in **English / Japanese / Korean** localization.

No accounts. No cloud. No tracking. Everything runs locally in the browser.

![preview](preview.png)

📖 **[User Manual](MANUAL.md)** · 🚀 **[Deploy guide](DEPLOY.md)**

## Features

- **Exact store dimensions** — iPhone 6.5″ (1284×2778), iPad 13″ (2064×2752), and Google Play
  Android phone (1080×1920), pixel-perfect.
- **No-alpha PNG export** — strips the alpha channel that browsers add by default, so App Store
  Connect won't reject your uploads. (JPEG export also available.)
- **EN / JA / KR localization** — write title/subtitle per language; Japanese and Korean render with
  bundled **Noto Sans JP / KR** fonts (no "tofu" boxes on any OS). Empty JA/KR fields fall back to EN.
  The **tool interface itself** also switches language with the tab (EN / 日本語 / 한국어).
- **Top-text layout** with gradient or solid backgrounds, adjustable text size, color, text area,
  corner radius, drop shadow, a **Fit / Fill** toggle for screenshot sizing (aspect ratio preserved),
  a **Trim top** control to cleanly remove the simulator status bar, and an optional generic
  **device frame** (a plain rounded bezel, no Apple artwork).
- **Up to 10 slides**, one screenshot set per device.
- **Instant start** — "Try a sample" loads a demo mockup + copy so you see a result immediately (no
  screenshots needed yet), and one-click style **Presets** give a polished look fast.
- **Save & resume** — "Save" exports your whole deck (text, screenshots, style) to a JSON file;
  "Load" re-opens it later to keep working or share it.
- **Clean 3-panel workspace** — content on the left, a large live preview in the middle, style on the
  right; a one-row header with a "More formats" menu for the non-phone sizes.
- **Style options** — gradient / solid / **image** backgrounds, a **logo overlay**, **frame colours**,
  and **independent title/subtitle fonts**. Plus a Google Play **feature graphic** (1024×500) banner.
- **Batch ZIP export** — "Export all" bundles every slide × device × language into a single
  `shotsmith_screenshots.zip`, foldered `{language}/{device}_{NN}.png` so each folder maps to App
  Store Connect's per-language slots. iPhone / iPad and EN / JA / KR checkboxes let you export just the
  device classes and languages you need (languages default to the ones you actually wrote text in).

## Usage

1. Open `index.html` (see "Running locally" below) or visit the hosted demo.
2. Pick a language tab (EN / 日本語 / 한국어) and type your title + subtitle.
3. Drag your iPhone and iPad screenshots into the two drop zones.
   - The same screenshot is reused across languages — only the text is translated.
4. Tweak background, colors, and layout to taste.
5. Click **Export all** to download one `shotsmith_screenshots.zip`, with files foldered as
   `{lang}/{device}_{NN}.png` (e.g. `ja/iphone_01.png`) — each folder maps to App Store Connect's
   per-language slots. (**Download current** still saves a single `{lang}_{device}_{NN}.png`.)

### A note on store copy
Apple requires overlay text to be **descriptive and factual**. Avoid superlatives, rankings,
awards, or competitor comparisons ("best", "#1", etc.) — they can get your screenshots rejected.

## Running locally

Because the fonts load via `@font-face`, open it through a local server (some browsers block
font loading from `file://`):

```bash
# from the project folder
python3 -m http.server 8000
# then open http://localhost:8000
```

## Hosting (GitHub Pages)

Push this folder to a repo and enable **Settings → Pages → Deploy from branch**.
Your tool will be live at `https://<user>.github.io/<repo>/`. No build step needed.

## Store screenshot specs (2026)

| Device class | Required size | Notes |
|---|---|---|
| iPhone 6.5″ | 1284×2778 | App Store Connect's iPhone slot (1242×2688 also accepted) |
| iPad 13″ | 2064×2752 | Required if your app supports iPad (2048×2732 also accepted) |
| Android phone | 1080×1920 | Google Play (9:16); Play accepts 320–3840px per side, ratio ≤2:1 |
| iPhone 6.7″ | 1290×2796 | Optional iOS size |
| Android tablet | 1600×2560 | Google Play tablet |
| Mac | 2880×1800 | Mac App Store (landscape) |
| Feature graphic | 1024×500 | Google Play listing banner — text/logo, no screenshot |
| Format | PNG or JPEG | **No alpha/transparency**, sRGB, 1–10 per device class |

Specs change as Apple adds devices — verify in App Store Connect before uploading.

## Tech

Single static page: vanilla JS + HTML Canvas, no dependencies, no backend.
PNG alpha-stripping uses `CompressionStream` (falls back to JPEG where unavailable).

## License

Code: **MIT** (see [LICENSE](LICENSE)).
Bundled fonts (Noto Sans JP / KR): **SIL Open Font License 1.1** (see [fonts/OFL.txt](fonts/OFL.txt)).

---

## 한국어 안내

브라우저에서 완전히 로컬로 동작하는 **App Store 스크린샷 도구**입니다. 시뮬레이터 스크린샷을
끌어다 놓고 타이틀·서브타이틀을 입력하면, **정확한 규격의 업로드용 파일**을 만들어 줍니다.
**영어·일본어·한국어** 로컬라이제이션을 기본 지원합니다(Noto Sans JP/KR 폰트 번들).

- iPhone 1284×2778 / iPad 2064×2752 정확한 픽셀 출력
- 브라우저가 넣는 알파 채널을 제거한 RGB PNG(Apple 리젝 방지) / JPEG 선택 가능
- 언어 탭에서 문구만 번역, 스크린샷은 공통 사용 (JA/KR 비면 EN으로 자동 대체)
- **Export all** → `shotsmith_screenshots.zip` 하나로 묶어 저장(`언어/기기_번호.png` 폴더 구조) → App Store Connect 언어별 슬롯에 그대로 사용

로컬 실행은 `python3 -m http.server` 로 띄운 뒤 `http://localhost:8000` 접속을 권장합니다
(폰트가 `file://`에서 로드되지 않는 브라우저가 있어서입니다).
