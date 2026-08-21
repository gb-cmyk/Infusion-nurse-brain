# Chair Board \u2014 deploy to Netlify

## What's in this package
- `index.html` \u2014 the entire app (no build step needed; it loads React from a CDN and runs in-browser)
- `manifest.json` \u2014 lets iOS treat it as an installable app icon
- `icon-192.png`, `icon-512.png` \u2014 app icon

## Deploy (2 minutes)
1. Go to https://app.netlify.com/drop
2. Drag the whole unzipped folder (or this zip file) onto the page
3. Netlify gives you a live URL immediately (something like `random-name-123.netlify.app`)
4. Optional: in Netlify's site settings you can rename the subdomain to something memorable, e.g. `your-name-chairboard.netlify.app`

## Put it on your iPhone home screen
1. Open the Netlify URL in **Safari** on your iPhone (must be Safari, not Chrome, for this to work)
2. Tap the Share icon \u2192 **Add to Home Screen**
3. It now opens full-screen, no browser bar, like a normal app

## Important notes
- **Data lives only on your phone**, in the browser's local storage. Nothing is sent to a server. Clearing Safari's site data, or iOS clearing storage under space pressure, can erase it \u2014 don't treat this as permanent record-keeping.
- **Timers now run in real time** (not the sped-up demo clock from the design preview).
- **Voice notes** use your browser's built-in speech recognition, which typically sends audio to a cloud service (Apple's/Google's) to transcribe \u2014 worth knowing if you're dictating anything sensitive.
- **Epic (or your EHR) remains the source of truth.** This is a personal shift tracker only.
- Every time you refresh Netlify's deployed version with an update, your saved patient data on your phone is untouched (it lives in the phone's browser storage, separate from the hosted files) \u2014 but a fresh browser or a different device will start empty.

## Making changes later
Since there's no build step, you can just edit `index.html` directly in any text editor and re-drag the folder to Netlify to redeploy \u2014 no npm, no compiling.
