# Deploy Runbook — tsyrenov1987.github.io

Goal: deploy this `site/` folder as the GitHub Pages user site at
`https://tsyrenov1987.github.io/`, then update App Store Connect's
Marketing URL so AdMob can verify `app-ads.txt`.

Total developer time: ~15 minutes active + 24-48h waiting for AdMob.

---

## A. Create the GitHub repository

1. Open <https://github.com/new>
2. **Repository name** (exactly — case sensitive):
   ```
   tsyrenov1987.github.io
   ```
   This MUST match your GitHub username, with `.github.io` suffix.
   GitHub treats this as your "User Site" repo — files in it serve at
   the bare domain root. **Any other name puts files in a subfolder
   and breaks AdMob's app-ads.txt crawl.**
3. Visibility: **Public** (required for free Pages hosting)
4. Initialize options: leave ALL unchecked (no README, no .gitignore,
   no license)
5. Click **Create repository**

## B. Push the site/ folder

### Option 1 — Command line (fastest)

```bash
cd "/Users/nikolaitsyrenov/Desktop/Magic Merge/site"
git init
git add .
git commit -m "Initial site + app-ads.txt for AdMob verification"
git branch -M main
git remote add origin git@github.com:tsyrenov1987/tsyrenov1987.github.io.git
git push -u origin main
```

If `git push` asks for credentials, you may need to set up an SSH key
or use HTTPS:
```bash
git remote set-url origin https://github.com/tsyrenov1987/tsyrenov1987.github.io.git
git push -u origin main
```

### Option 2 — Web upload (no terminal)

1. On the empty repo page, click **uploading an existing file**
2. Drag the **contents** of `site/` (not the folder itself — open the
   folder and select all 5 files including the hidden `.nojekyll`)
3. Commit message: `Initial site + app-ads.txt for AdMob verification`
4. Click **Commit changes**

⚠️ If `.nojekyll` is hidden in Finder, press `Cmd+Shift+.` to
show hidden files before drag-drop.

## C. Enable GitHub Pages

1. Repo → **Settings** (top tab) → **Pages** (left sidebar)
2. **Source**: "Deploy from a branch"
3. **Branch**: `main` · `/ (root)` → **Save**
4. Wait 30–60 seconds; refresh — you should see:
   > Your site is live at `https://tsyrenov1987.github.io/`

## D. Verify the deployment

Open Terminal and run:

```bash
curl -sI https://tsyrenov1987.github.io/app-ads.txt | head -3
echo "---"
curl -s https://tsyrenov1987.github.io/app-ads.txt
echo "---"
curl -sI https://tsyrenov1987.github.io/ | head -3
```

Expected output:

```
HTTP/2 200
content-type: text/plain; charset=utf-8
...
---
google.com, pub-3115922230705948, DIRECT, f08c47fec0942fa0
---
HTTP/2 200
content-type: text/html; charset=utf-8
...
```

If `app-ads.txt` returns 404, wait another 2 minutes and retry —
Pages sometimes takes longer on first deploy.

Also open in a browser:
- <https://tsyrenov1987.github.io/> — landing
- <https://tsyrenov1987.github.io/support.html> — support
- <https://tsyrenov1987.github.io/app-ads.txt> — raw text

## E. App Store Connect — update Marketing URL only

1. Open <https://appstoreconnect.apple.com/apps/6772005772>
2. Left sidebar → **App Information**
3. Scroll to **General Information**:
   - **Marketing URL**: `https://tsyrenov1987.github.io/`  ← NEW, this
     is what AdMob will crawl
   - **Support URL**: keep `mailto:tsyrenov1987@gmail.com` OR change to
     `https://tsyrenov1987.github.io/support.html` (your choice)
   - **Privacy Policy URL**: **DO NOT CHANGE** — keep
     `https://telegra.ph/Privacy-Policy--Magic-Merge-05-22`
4. Click **Save** (top right)

URL changes do NOT trigger App Review. The change propagates to the
public App Store listing within minutes.

## F. AdMob console — trigger app-ads.txt crawl

1. Open <https://apps.admob.com>
2. Sign in with **tsyrenov1987@gmail.com**
3. Left sidebar → **Apps** → click **Magic Merge**
4. Check **Settings** (gear icon next to app name):
   - Confirm "App store URL" field matches the public App Store listing
     URL: `https://apps.apple.com/us/app/magic-merge/id6772005772`
5. Top right → profile icon menu → **App-ads.txt**
6. Find Magic Merge in the list and click **Recheck** / **Crawl now**
7. Status changes to **Authorized** within 24–48 hours

While you wait, you can verify the crawl will succeed by checking
the AdMob expected URL yourself:

```bash
curl -v https://tsyrenov1987.github.io/app-ads.txt
```

If you see `HTTP/2 200` and the correct content, AdMob will see the
same.

## G. Expected timeline

| Step | When |
|---|---|
| Repo created + files pushed | T+0 (now) |
| Pages live at github.io | T+1 min |
| App Store Connect Marketing URL updated | T+2 min |
| App Store listing reflects new URL | T+5–15 min |
| AdMob "Recheck" triggered | T+5 min |
| AdMob status → Authorized | T+24–48h |
| Ad serving begins | After Authorized |
| Fill rate ramps up (new app) | 1–2 weeks |

---

## Risks / gotchas

1. **Repo name MUST be `tsyrenov1987.github.io` exactly.** Any other
   name (e.g. `magic-merge`, `magicmerge-site`) puts files at
   `https://tsyrenov1987.github.io/repo-name/` — AdMob won't find
   `app-ads.txt` at the root and verification fails silently.
2. **`app-ads.txt` is case-sensitive.** Always lowercase, with hyphen.
3. **No CRLF line endings** in `app-ads.txt` — some crawlers reject
   them. The file is already saved with LF; don't open and re-save it
   in a Windows editor.
4. **Don't touch Privacy Policy URL** unless you're migrating the
   privacy policy off telegra.ph. The current URL works fine.
5. **HTTP 301/302 redirects** are tolerated by AdMob but a direct 200
   is safer. Pages serves directly — no redirects expected.
6. **Custom domain later** (e.g. `magicmerge.app`): add a `CNAME` file
   to the repo root containing just the domain, configure DNS, then
   re-run AdMob recheck against the new root. The github.io URL will
   keep working as a fallback.
7. **Adding analytics later** will require a separate decision —
   currently this site has zero trackers and zero JavaScript, which
   keeps privacy clean and avoids any AdMob policy issues around
   tracking on the developer's own site.

---

## What this does NOT change

- Your Reddit r/iosgaming plan
- Your featuring nomination draft in App Store Connect
- The privacy policy on telegra.ph
- The App Store listing screenshots, description, IAPs, or pricing
- Any code in the iOS app

Only the website + 1 URL field in App Store Connect changes.
