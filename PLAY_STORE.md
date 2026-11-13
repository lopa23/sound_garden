# Publishing Sound Garden to Google Play

The app is a PWA (installable, works offline) at https://lopa23.github.io/sound_garden/.
It reaches Google Play as a **Trusted Web Activity (TWA)**: a small Android wrapper that opens the site full-screen.

## Done
- [x] Web app manifest, icons (192, 512, maskable), offline service worker
- [x] Privacy policy: https://lopa23.github.io/sound_garden/privacy.html (no data collected, no ads, no mic)

## Steps

1. **Create a Google Play developer account** (you): https://play.google.com/console/signup
   - One-time US$25 fee and identity verification.
   - New *personal* accounts must run a **closed test with at least 12 testers for 14 days** before they can publish to production. Line up 12 friends or family with Android phones.

2. **Generate the Android package** at https://www.pwabuilder.com
   - Enter `https://lopa23.github.io/sound_garden/`, then choose **Package for stores → Android**.
   - Package ID: `io.github.lopa23.soundgarden`. App name: `Sound Garden`.
   - Download the zip. It contains the `.aab` (upload this to Play), the **signing key (`signing.keystore`) and its passwords**, and `assetlinks.json`.
   - **Back up the keystore and passwords somewhere safe.** Every future update must be signed with the same key.

3. **Prove that you own the site (Digital Asset Links)**
   - Android only hides the browser address bar if it finds `assetlinks.json` at the **domain root**: `https://lopa23.github.io/.well-known/assetlinks.json`.
   - The app currently lives under `/sound_garden/`, so this needs a separate repo named **`lopa23.github.io`** containing `.well-known/assetlinks.json` (plus an empty `.nojekyll` file so GitHub serves the dot-folder).
   - After the first upload, add Google Play's **App signing key** SHA-256 fingerprint to that file as well (Play Console → Setup → App signing).

4. **Fill in the Play Console** (you)
   - **Store listing:** short description (80 chars), full description, 512×512 icon (`icons/icon-512.png`), a 1024×500 feature graphic, and at least 2 phone screenshots.
   - **Privacy policy URL:** the link above.
   - **Target audience:** choose ages **5 and under / 6–8**. This puts the app under Google's **Families policy**: no ads SDKs and no data collection, which the app already meets.
   - **Data safety:** "No data collected" and "No data shared".
   - **Content rating questionnaire:** educational, no violence, no user interaction. This should rate *Everyone*.
   - **App category:** Education.

5. **Release:** upload the `.aab` to **Closed testing**, add the testers, wait 14 days, then apply for **Production**.

## Updating later
Changes pushed to GitHub show up in the app automatically, because it loads the live site. The service worker refreshes cached files in the background, so updates appear on the next launch. Bump `CACHE` in `sw.js` for big changes. You only rebuild and re-upload the `.aab` if the name, icon or package settings change.
