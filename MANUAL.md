# Shotsmith — User Manual

A lightweight, local-first tool for making **App Store screenshots** right in your browser.
Drop in your simulator captures, add a headline per language, and export store-ready images
at the exact sizes App Store Connect requires.

---

## 1. What it is

Shotsmith turns raw app screenshots into polished App Store screenshots:

- Puts a **title + subtitle** above your screenshot on a colored background.
- Exports at **exact App Store dimensions** (iPhone 1284×2778, iPad 2064×2752).
- Produces **alpha-free PNG** files (the kind App Store Connect accepts), or JPEG.
- Supports **English / Japanese / Korean** — text *and* the interface.

Everything runs locally in your browser. No account, no upload, no tracking.

---

## 2. Requirements

- A modern browser (Chrome, Safari, Edge, or Firefox).
- Your app screenshots, captured from the iOS Simulator or a real device
  (one set for iPhone, one set for iPad).

---

## 3. Opening the tool

**Easiest:** visit the hosted version (your GitHub Pages link, if you deployed it).

**Locally:** because fonts load via `@font-face`, open it through a tiny local server so
the browser is allowed to load them:

```bash
cd shotsmith
python3 -m http.server 8000
# then open http://localhost:8000 in your browser
```

(Opening `index.html` directly with `file://` works in most browsers, but a few block local
font loading — the local server avoids that entirely.)

---

## 4. Interface overview

**Top bar**
- **iPhone / iPad** — switch which device you're previewing and exporting.
- **PNG / JPEG** — output format. PNG is alpha-free and best for text; JPEG is also accepted.
- **Download current** — saves just the slide/device/language you're viewing.
- **Export all** — bundles every slide × device × language into a single `shotsmith_screenshots.zip`.
  Use the **iPhone / iPad** checkboxes beside it to include only the device class(es) you need.

**Left panel — Slide content**
- **EN / 日本語 / 한국어 tabs** — pick the language you're editing. The whole interface and the
  title/subtitle fields switch to that language. A green dot marks tabs that already have text.
- **Title / Subtitle** — your headline and supporting line for the selected language.
- **iPhone shot / iPad shot** — drag an image in, or click to browse. The same screenshot is
  reused across languages (only the text is translated).

**Slides** — thumbnails of each slide (max 10). Click to select; click × to delete; **+ Add slide**
to add. The two dots show whether an iPhone / iPad screenshot is loaded.

**Background** — Gradient (top + bottom color) or Solid (one color).

**Text** — title/subtitle color, the Latin font for English, and title/subtitle sizes.
Japanese uses Noto Sans JP and Korean uses Noto Sans KR automatically.

**Layout**
- **Text area** — how much vertical space the headline reserves at the top (Fit mode).
- **Corner radius** — roundness of the screenshot's corners.
- **Trim top** — crops the top of the screenshot. Use ~3–4% to remove the simulator status bar
  (clock / battery). The rounded corners reapply to the new top edge automatically.
- **Screenshot: Fit / Fill** — *Fit* keeps a margin around the screenshot; *Fill* enlarges it to
  nearly fill the area below the text (aspect ratio preserved, headline never covered).
- **Shadow** — drop shadow under the screenshot.

---

## 5. Quick start: your first screenshot set

1. Capture screenshots from the iOS Simulator (one set for iPhone, one for iPad).
2. Open Shotsmith. Leave the language on **EN**.
3. Type a **Title** and **Subtitle** describing the screen.
4. Drag your iPhone screenshot into **iPhone shot**, and your iPad screenshot into **iPad shot**.
5. Adjust **Background** colors and, if you see a status bar, raise **Trim top** to ~3–4%.
6. Add more slides with **+ Add slide** and repeat (up to 10).
7. Click **Export all** — you get one `shotsmith_screenshots.zip`. Unzip it and drag each language
   folder's files into App Store Connect.

---

## 6. Working in multiple languages (EN / JA / KR)

1. Fill in the English text first.
2. Click the **日本語** tab and type the Japanese title/subtitle; same for **한국어**.
3. Screenshots are shared — you don't re-upload them per language.
4. If you leave a Japanese or Korean field empty, that screenshot falls back to the English text,
   so a half-translated set still exports complete images.
5. **Export all** outputs only the languages that have text (English is always included), as one
   `shotsmith_screenshots.zip` foldered by language — `en/iphone_01.png`, `ja/iphone_01.png`,
   `ko/ipad_02.png`, and so on.

In App Store Connect, each language has its own screenshot slots — unzip and drop each `xx/…`
folder's files into the matching language.

---

## 7. Exporting & file names

- **Format:** PNG (alpha-free, lossless, best for UI text) or JPEG (smaller, also accepted).
- **Download current:** one file for the current slide/device/language, named
  `{language}_{device}_{NN}.{ext}` — e.g. `ja_ipad_03.png`.
- **Export all:** the full matrix as a single `shotsmith_screenshots.zip` (one download, no
  multi-download prompt), with files foldered by language: `{language}/{device}_{NN}.{ext}` —
  e.g. `ja/iphone_01.png`. Tick only **iPhone** or only **iPad** beside the button to export a
  single device class (e.g. when you're shipping iPhone-only) — the other one is left out entirely.

---

## 8. Writing good copy (important)

Apple requires screenshot text to be **descriptive and factual**. Avoid superlatives, rankings,
awards, and competitor comparisons ("best", "#1", "award-winning", "better than…") — these can
get your screenshots rejected. Describe what the screen does instead.

---

## 9. Troubleshooting

- **Japanese/Korean shows as boxes (□□□):** you opened `index.html` from `file://` and the browser
  blocked font loading. Run it via `python3 -m http.server` (see §3), or use the hosted version.
- **Upload rejected for transparency/alpha:** make sure you exported with this tool (it strips the
  alpha channel). If your browser lacks `CompressionStream`, switch to **JPEG**.
- **Wrong size error in App Store Connect:** confirm the device toggle matches the slot
  (iPhone 6.5″ = 1284×2778, iPad 13″ = 2064×2752).
- **Status bar still visible:** raise **Trim top** a little more.

---

## 한국어 안내

브라우저에서 로컬로 동작하는 App Store 스크린샷 도구입니다. 시뮬레이터 캡처를 끌어다 놓고,
언어별 타이틀/서브타이틀을 입력하면, App Store Connect가 요구하는 정확한 규격의 이미지를 만들어 줍니다.

**기본 사용 순서**
1. 시뮬레이터에서 iPhone용·iPad용 스크린샷을 각각 캡처합니다.
2. 도구를 엽니다(로컬은 `python3 -m http.server 8000` 후 `http://localhost:8000` 권장 — 폰트 로딩 때문).
3. EN 탭에서 타이틀·서브타이틀을 입력하고, 두 드롭존에 iPhone/iPad 스크린샷을 넣습니다.
4. 상태바(시간·배터리)가 보이면 **Trim top**을 3~4%로 올려 잘라냅니다.
5. **日本語 / 한국어** 탭에서 번역 문구를 입력합니다(스크린샷은 공통, 비우면 EN으로 자동 대체).
6. 배경색·레이아웃을 다듬고 **+ Add slide**로 슬라이드를 추가합니다(최대 10장).
7. **Export all**을 누르면 `shotsmith_screenshots.zip` 하나로 묶여 저장됩니다(`언어/기기_번호.png` 폴더 구조).
   압축을 풀어 언어별 폴더의 파일을 App Store Connect 언어 슬롯에 넣으세요.

**주의:** 오버레이 문구는 사실적 톤으로. "최고/1위/수상/경쟁사 비교"는 Apple 리젝 사유가 될 수 있습니다.

**문제 해결:** 한글·일본어가 □□□로 보이면 `file://`에서 폰트가 막힌 것이니 로컬 서버로 여세요.
업로드가 투명도(알파)로 거부되면 이 도구로 내보낸 PNG인지 확인하고, 안 되면 JPEG로 전환하세요.
