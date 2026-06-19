# Publishing Shotsmith on GitHub (step by step)

This guide takes you from the files on your computer to a live, public web tool that anyone can use.
Two paths are given — pick **A (website, no command line)** or **B (command line)**.

---

## 0. Before you start (1 minute)

- Open **LICENSE** and replace `[Your Name]` with your name. Save.
- Have a free GitHub account. If you don't, create one at https://github.com (this is the only step
  that needs your own login — for security, do it yourself).
- Decide a repository name, e.g. `shotsmith`. Your live URL will be
  `https://<your-username>.github.io/shotsmith/`.

---

## Path A — GitHub website (no command line)

1. Go to https://github.com/new
2. **Repository name:** `shotsmith`. **Visibility:** Public. Leave "Add a README" **unchecked**
   (you already have one). Click **Create repository**.
3. On the new empty repo page, click the link **“uploading an existing file.”**
4. Open your `shotsmith` folder, select **all of its contents** —
   `index.html`, the `fonts` folder, `README.md`, `MANUAL.md`, `LICENSE`, `preview.png` — and
   **drag them into the upload box**. (Folder drag-and-drop is supported.)
5. Scroll down and click **Commit changes**.
6. Now enable the website: go to **Settings → Pages**. Under **Build and deployment → Source**,
   choose **Deploy from a branch**. Set **Branch: `main`** and **folder: `/ (root)`**. Click **Save**.
7. Wait about a minute, then reload the Pages settings page. A green banner shows your live link:
   `https://<your-username>.github.io/shotsmith/`. Open it — you're live.

---

## Path B — Command line (git or GitHub CLI)

### Easiest: GitHub CLI (creates the repo and pushes in one command)

```bash
# one-time: install + sign in
brew install gh          # macOS (Homebrew)
gh auth login            # follow the prompts (opens a browser to authorize)

# from inside the shotsmith folder:
cd ~/Downloads/shotsmith
gh repo create shotsmith --public --source=. --remote=origin --push
```

### Or plain git (create the empty repo on the website first, then):

```bash
cd ~/Downloads/shotsmith
git init
git add .
git commit -m "Initial release: Shotsmith"
git branch -M main
git remote add origin https://github.com/<your-username>/shotsmith.git
git push -u origin main
```

> Note: GitHub no longer accepts your account password on the command line. Use **`gh auth login`**
> (recommended), an **SSH key**, or a **Personal Access Token** as the password when pushing.

### Then enable Pages (same as Path A, step 6–7)

Settings → Pages → Deploy from a branch → `main` / `(root)` → Save → wait ~1 min.

---

## Verify

- Visit `https://<your-username>.github.io/shotsmith/`.
- Switch the EN / 日本語 / 한국어 tabs — the interface should change language.
- Drop in a test screenshot and click **Export all** to confirm files download.

---

## Updating later

**Website:** open the repo → **Add file → Upload files** → drag the changed file(s) → Commit.
The live site updates within a minute.

**Command line:**
```bash
git add .
git commit -m "Describe your change"
git push
```

---

## 한국어 안내 (요약)

0. **LICENSE**의 `[Your Name]`을 본인 이름으로 바꿔 저장. GitHub 계정 준비(없으면 직접 가입 — 로그인은 본인만 가능).
1. **방법 A (웹, 명령어 없음):** github.com/new → 이름 `shotsmith`, Public, README 체크 해제 → Create
   → "uploading an existing file" 클릭 → `shotsmith` 폴더 **내용물 전체**를 드래그 → Commit.
2. **웹사이트 켜기:** Settings → Pages → Source "Deploy from a branch" → `main` / `(root)` → Save
   → 약 1분 후 `https://<아이디>.github.io/shotsmith/` 링크 생성.
3. **방법 B (명령어):** `gh auth login` 후 폴더에서
   `gh repo create shotsmith --public --source=. --remote=origin --push` 한 줄이면 생성+푸시 완료.
   이후 Pages만 켜면 됩니다(2번과 동일).
4. **수정 배포:** 웹은 Upload files로 덮어쓰기, 명령어는 `git add . && git commit -m "..." && git push`.

> 참고: GitHub는 명령어에서 계정 비밀번호를 더 이상 받지 않습니다. `gh auth login`(권장)이나 SSH 키,
> 또는 Personal Access Token을 사용하세요.
