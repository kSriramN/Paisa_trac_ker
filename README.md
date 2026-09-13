# Paisa Tracker — PWA → Android APK

This folder has everything PWABuilder needs:
- `index.html` — the app (manifest + service worker already wired in)
- `manifest.json`
- `sw.js` — service worker, caches the app so it works fully offline
- `icons/` — app icons (192, 512, maskable, apple-touch)

## Important: PWABuilder needs a live HTTPS URL, not local files

You can't point PWABuilder at files on your computer/phone — it fetches your
manifest and service worker over the internet, so you need to host this
folder somewhere with HTTPS first. Easiest free options, no account
juggling required:

1. **GitHub Pages** (recommended, free, permanent)
   - Create a new GitHub repo, upload everything in this folder to it
     (keep the folder structure — `icons/` must stay a subfolder)
   - Repo Settings → Pages → set source to your main branch
   - You'll get a URL like `https://yourusername.github.io/paisa-tracker/`

2. **Netlify Drop** (fastest, no account needed)
   - Go to https://app.netlify.com/drop
   - Drag this whole folder in — it gives you a live HTTPS URL in seconds

Either way, once it's hosted, open the URL in a browser first and check
`yoururl/manifest.json` loads correctly — that confirms it's set up right
before you hand it to PWABuilder.

## Generating the APK

1. Go to https://www.pwabuilder.com
2. Paste your hosted URL (e.g. `https://yourusername.github.io/paisa-tracker/`)
   and click **Start**
3. It'll scan the manifest + service worker and show a score — you should
   see green checks for manifest and service worker with the files above
4. Click **Package for stores** → **Android**
5. Leave the defaults (Trusted Web Activity) unless you know you want to
   change the package name — by default it'll be something like
   `com.yourdomain.paisatracker`; feel free to edit it before downloading
6. Download the generated package — you'll get an `.apk` (or `.aab` for
   Play Store) you can install directly on an Android phone

## Notes

- All your data (transactions, cards, budgets, saved statement analyses)
  lives in the phone's local storage inside the app — nothing is sent to
  a server. That also means: if you ever uninstall the app or clear its
  storage, use the in-app **Export** button first to back up your data.
- If you update `index.html` later, bump `CACHE_VERSION` at the top of
  `sw.js` (e.g. `paisa-tracker-v2`) so installed copies of the app pick up
  the change instead of serving the old cached version.
- The manifest's `start_url` and `scope` assume the app sits at the root
  of wherever you host it (e.g. `https://yoursite.com/`). If you host it
  in a subfolder (e.g. GitHub Pages project sites often do
  `https://user.github.io/repo-name/`), update `start_url`, `scope` in
  `manifest.json`, and the `sw.js` registration path (`sw.js` →
  `./sw.js` already works either way) accordingly — GitHub Pages project
  URLs work fine as long as the relative paths stay relative, which they
  already are here.
