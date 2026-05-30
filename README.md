# tsyrenov1987.github.io

Personal landing + app-ads.txt verification for Magic Merge
(App Store ID 6772005772, AdMob publisher pub-3115922230705948).

## Files

- `app-ads.txt` — AdMob verification (IAB Tech Lab spec)
- `index.html` — landing page
- `support.html` — support / FAQ (replaces mailto: support URL)
- `.nojekyll` — disables Jekyll build pipeline, faster builds

Privacy policy lives on telegra.ph and is linked from index.html
footer; not hosted in this repo.

## Deploy

Push to `main` → GitHub Pages rebuilds in ~30 sec.

## Verification

After deploy:

```
curl https://tsyrenov1987.github.io/app-ads.txt
```

Expected:
```
google.com, pub-3115922230705948, DIRECT, f08c47fec0942fa0
```
