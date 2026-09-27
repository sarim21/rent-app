# Rent Accounts — self-hosted app, APK & Google Drive sync

This folder is a complete, self-contained web app: `index.html`, `manifest.json`,
`sw.js` (offline support), and two icons. It works standalone with no setup —
just open `index.html` in a browser. The steps below are only needed for the
two extra things you asked for: a real `.apk` file, and Google sign-in sync.

## Part 1 — Host it somewhere (needed for both APK and Drive sync)

Both an APK and Google Sign-In require the app to live at a real public
`https://` address — a file opened directly from your phone's storage, or the
Claude artifact link, won't work for either. The free, no-signup-hassle option:

**GitHub Pages**
1. Create a free GitHub account if you don't have one.
2. Create a new repository (e.g. `rent-accounts`), and upload all 5 files
   from this folder to it (drag-and-drop works on github.com).
3. In the repo, go to **Settings → Pages**, set Source to your main branch,
   and save. GitHub gives you a URL like
   `https://yourname.github.io/rent-accounts/`.
4. Open that URL — it's now a real hosted PWA.

(Netlify Drop or Vercel work the same way if you'd rather use those.)

## Part 2 — Turn it into an installable APK

Once hosted, go to **[pwabuilder.com](https://www.pwabuilder.com)**, paste
your GitHub Pages URL, and click "Start". PWABuilder reads your
`manifest.json` and packages an **Android package (.apk / .aab)** for you —
free, no code. Download the APK, transfer it to your phone, and install it
(you'll need to allow "install from unknown sources" once, since it's not
from the Play Store). If you want it on the Play Store properly, PWABuilder
also gives you a signed `.aab` ready to upload to the Play Console (Google
charges a one-time $25 developer fee for that step — optional).

Simpler alternative if you don't need a literal file: once hosted, opening
the URL on Android Chrome and tapping **"Add to Home Screen"** installs it
exactly like an app, with its own icon, offline support, and no APK needed.

## Part 3 — Google Sign-In + Drive sync

This lets you sign into the same Google account on any device and pull the
same data down. It needs a free Google Cloud project (5 minutes):

1. Go to **[console.cloud.google.com](https://console.cloud.google.com)**,
   create a new project (any name).
2. **APIs & Services → Library** → search "Google Drive API" → Enable it.
3. **APIs & Services → OAuth consent screen** → choose "External", fill in
   an app name and your email, save (you can leave it in "Testing" mode and
   add your own Google account under "Test users" — no Google review needed
   for personal use).
4. **APIs & Services → Credentials → Create Credentials → OAuth client ID**
   → Application type: **Web application**.
   - Under "Authorized JavaScript origins", add your GitHub Pages URL
     (e.g. `https://yourname.github.io`) — no trailing slash or path.
5. Copy the generated **Client ID** (ends in `.apps.googleusercontent.com`).
6. Open `index.html` in a text editor, find this line near the top of the
   `<script>` section:
   ```js
   const GOOGLE_CLIENT_ID = 'YOUR_CLIENT_ID.apps.googleusercontent.com';
   ```
   and paste your real Client ID in place of the placeholder. Re-upload the
   edited file to your GitHub repo.
7. Open your hosted URL again → go to the **Account** tab → "Sign in with
   Google". On sign-in, if data already exists in that Google account's
   Drive, it'll offer to load it onto the current device. Use "Save this
   device's data to Drive" any time to push the current device's data up.

The data is stored in a private file Google calls the "app data folder" —
it's tied to your Google account and this app, but won't show up in your
normal Drive file list. Nothing else about your Drive is touched.

### What this does *not* do
- It's not automatic background sync — you tap "Save to Drive" / "Check
  Drive" when you want to sync, since a browser page can't run unattended.
- Each browser session needs you to sign in again after about an hour
  (a standard limit for this kind of client-only setup, since there's no
  server to keep you signed in longer). This is normal for apps built this
  way without a backend.
