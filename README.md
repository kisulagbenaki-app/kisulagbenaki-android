# কিছু লাগবে নাকি? — Android TWA

**Package:** `com.kisulagbenaki.app`  
**Website:** https://kisulagbenaki-app.github.io/kisulagbenaki-android/

This is a standalone Android Studio project. It does not replace your existing website's `index.html`.

## Android Studio
1. Open the root folder `kisulagbenaki-twa-project`.
2. Install Android SDK 35, JDK 17 and Gradle 8.10.2 (Android Studio can sync using the Gradle configuration).
3. Sync Gradle and select **Build > Build APK(s)** to produce a debug APK.
4. For Play Store, create a persistent release/upload signing key and configure signing. Never commit the keystore or passwords.

## GitHub Actions
Upload the **contents of this project** to your `kisulagbenaki-android` repository root (keep existing website `index.html` unchanged). GitHub Actions -> Build Android TWA -> Run workflow. It creates a debug APK and an AAB artifact. **Without signing secrets the AAB is unsigned and cannot be uploaded to Play Store.**

To sign the AAB, set repository Actions secrets:
- `ANDROID_KEYSTORE_BASE64`: base64-encoded upload key JKS (one-line)
- `ANDROID_KEYSTORE_PASSWORD`
- `ANDROID_KEY_ALIAS`
- `ANDROID_KEY_PASSWORD`

Keep a secure offline backup of your upload key.

## Digital Asset Links — REQUIRED for full-screen TWA
Replace the placeholder SHA-256 in `website/.well-known/assetlinks.json` with the certificate fingerprint of the **actual installed app**. For Play App Signing use the **App signing key certificate SHA-256** from Play Console for Play-distributed installations; for locally installed builds use the local signing certificate. You may include multiple fingerprints.

Publish it to **the root of the GitHub Pages hostname**:
`https://kisulagbenaki-app.github.io/.well-known/assetlinks.json`

**Important GitHub Pages constraint:** your current project site is served at `/kisulagbenaki-android/`. Placing `assetlinks.json` inside that project repository's `.well-known` folder only publishes it at `/kisulagbenaki-android/.well-known/assetlinks.json`, which is **NOT sufficient** for origin-wide Android verification. To publish at the hostname root, create a separate public repository named `kisulagbenaki-app.github.io` under the same GitHub account and publish `.well-known/assetlinks.json` from that user-site repository (enable Pages on main/root). The user-site repository can coexist with your project-site repository. Alternatively use a custom domain whose root you control.

Check `https://kisulagbenaki-app.github.io/.well-known/assetlinks.json` returns 200 with the final real fingerprint. Until verified, Chrome may show its toolbar instead of full-screen TWA.

## Before Play Store submission
- Replace placeholder launcher artwork with final approved brand assets.
- Provide privacy policy, support contact, Data Safety declaration and appropriate disclosures/permissions.
- Confirm website features work and satisfy Play Store minimum functionality and webview/TWA policy requirements.
- Test on a real Android device; test browser support and offline/network failure handling.
- Check current Play Console target SDK requirements at release time.

**Not included:** Firebase backend, phone OTP, push notifications, voice assistant native permissions or actual payments. These require separate integration and verification.
