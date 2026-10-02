# Rent Accounts – deploy + Google sign-in

## A. Put it online (Netlify)
1. Go to app.netlify.com/drop and drag the whole folder (or the zip) in.
2. Note your address, e.g. https://your-name.netlify.app (HTTPS is required for sign-in and PWA).

## B. Get a Google Client ID (one time, free)
1. console.cloud.google.com → create a project.
2. APIs & Services → Library → enable **Google Drive API**.
3. OAuth consent screen → External → app name, your email → Save.
   Scopes used (drive.appdata, email, profile, openid) are non-sensitive, so no Google verification is needed.
   Click **Publish app** (otherwise only added Test users can sign in).
4. Credentials → Create credentials → OAuth client ID → **Web application**.
   Authorized JavaScript origins: your Netlify URL (no trailing slash) and your custom domain if any. Copy the Client ID.
5. Open the app → menu (More) → **Account** → paste the Client ID → Save → Sign in with Google.
   (Or paste it into `DEFAULT_CID` near the bottom of index.html so every device gets it automatically.)

## C. Use on several devices
Open the same site on each device, More → Account, sign in with the same Gmail.
Changes upload about 2 seconds after you save; other devices pull the latest when the app is opened or brought to the foreground.
If both sides changed, the app asks which version to keep.

## D. Install / publish with PWA Builder
1. pwabuilder.com → enter your Netlify URL → it should detect the manifest, service worker and icons.
2. Android: Package for stores (Trusted Web Activity) – sign-in works because it runs in Chrome. Add the same HTTPS origin in Google Cloud.
3. iOS / Windows packages run inside a web view, and Google blocks sign-in there. Use the site in Safari/Edge (Add to Home Screen), or rely on manual .json backup in those packages.
