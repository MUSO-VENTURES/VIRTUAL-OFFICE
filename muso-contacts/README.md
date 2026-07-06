# MUSO Contact Assistant — Offline App

A standalone, installable web app version of the New Contact Assistant. Works fully offline on your iPad once installed; no Cowork session or internet connection needed to view contacts, scan cards, or draft follow-ups.

## What's different from the Cowork artifact

This version has no live connection to your Gmail or Google Drive account (a self-hosted static site can't use those Cowork-only connectors). Instead:

- **Directory** is seeded from a snapshot of MUSO_Contacts.xlsx (`seed-contacts.json`, captured 2026-07-05). It won't auto-update when the real sheet changes — re-export and redeploy if you want a fresher snapshot.
- **New contacts** save to the device (localStorage) and go into a **Sync queue**.
- **Sync tab** gives you one block of tab-separated rows to copy and paste into MUSO_Contacts.xlsx once you're back on wifi. There's no automatic write to Google Sheets — no connector exists yet that can do that.
- **Business card scanning** uses on-device OCR (Tesseract.js) instead of Claude — it's a rougher first pass, so double-check names/numbers it fills in. The OCR engine downloads the first time you use it online, then is cached for offline use afterward.
- **Follow-up email** is a fill-in-the-blank template (no AI backend to call), with "Copy Text" and "Open in Mail App" options.

## Hosting it yourself

This is a fully static site — five files, no build step, no server code. Any static host works. A few easy options:

1. **GitHub Pages** (free, simplest for "real URL + HTTPS," which iOS requires for install-to-homescreen and offline caching):
   - Create a new GitHub repo, upload this folder's contents to it.
   - In repo Settings → Pages, set the source to the main branch root.
   - You'll get a URL like `https://yourname.github.io/muso-contacts/`.

2. **Netlify or Vercel drag-and-drop**: both let you drag this folder onto their dashboard and get an HTTPS URL in seconds, no account setup beyond signing in.

3. **Any existing web host / S3 bucket / your own server**: just upload the folder as-is — it's plain HTML/CSS/JS.

Avoid opening `index.html` directly from disk (`file://`) on the iPad — Safari won't register the service worker or allow "Add to Home Screen" to behave as a real app from a local file. It needs to be served over `https://`.

## Installing on iPad

1. Open the hosted URL in Safari.
2. Tap the Share icon → **Add to Home Screen**.
3. Launch it from the home screen icon — it now runs full-screen like a native app and works with airplane mode on, as long as you've opened it at least once while online (so the service worker can cache everything, including the OCR engine).

## Files

- `index.html` — the app
- `manifest.webmanifest` — PWA metadata (name, icons, theme color)
- `sw.js` — service worker; caches the app shell and any CDN assets it fetches, so it works offline after first load
- `seed-contacts.json` — starting contact data
- `icons/` — app icons (matches the MUSO Ventures brand)
