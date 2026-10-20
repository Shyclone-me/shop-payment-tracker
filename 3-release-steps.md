# Release steps: from this project to the Play Store

What was changed in the project for Play (already done, in the source zip):
- app.json: Android permissions the app does not use are blocked (internet, read and write storage, system alert window, vibrate), cloud backup is off (allowBackup false), a leftover Replit URL was removed from the expo-router plugin.
- eas.json: version codes are managed by EAS and increase automatically on production builds; the preview APK still builds arm64 only.
- app.plugin.js: production builds include 32-bit and 64-bit ARM (a Play bundle sends each phone only its own part, so size is not an issue and older phones can install). Set SHOPVOICE_ABIS to change this.
- lib/i18n.ts: the SMS permission screen now also says that the last 7 days are looked through once to list senders and that the list is not saved (Google compares this text with how the permission is used). English and Nepali.
- Checked here: expo prebuild produces a manifest with only READ_SMS and RECEIVE_SMS, allowBackup false, target API 36 (Google requires 36 or higher from 31 August 2026); the i18n test passes (115 keys). NOT checked here: an actual Gradle build, the final merged manifest, anything on a phone.

## Step 0. Test on a real phone first
1. `cd artifacts-shopvoice && eas build --platform android --profile preview` and install the APK.
2. Work through ANDROID-QA.md. Do not skip the privacy check (an SMS from an unticked sender must leave no trace) and the voice checks. Nothing in the native part has been run yet.

## Step 1. Accounts and hosting
1. Create a Google Play developer account (one-time fee, identity verification). Nepal supports developer registration; it does not support merchant registration, so you can publish free apps but not sell paid apps or in-app purchases from Nepal.
2. If it is a personal account created after 13 November 2023, Google requires a closed test with at least 12 testers, each opted in for 14 continuous days, before you can apply for production access. Start collecting 12+ testers (classmates, family, shopkeepers) with Gmail addresses early.
3. Host privacy-policy.html at a public URL. Easiest: a GitHub repository with Pages enabled, upload the file as index.html. Fill in your name and contact email and the date first. The URL goes into the store listing and the Data safety form.

## Step 2. Build the release bundle
1. `npm install -g eas-cli` then `eas login`.
2. In artifacts-shopvoice: `eas build --platform android --profile production`. Let EAS create and keep the signing key.
3. Download the .aab. Before uploading, check its permissions (bundletool is a Google tool you can download): `java -jar bundletool.jar dump manifest --bundle app.aab | grep uses-permission`. Expect only READ_SMS and RECEIVE_SMS (plus none for internet). If anything else appears, tell me and we fix it before uploading, because the Data safety form and the privacy policy say there is no internet permission.

## Step 3. Create the app in Play Console
1. Create app: name Shop Payment Tracker, default language English (United Kingdom or United States), App, Free.
2. Opt in to Play App Signing when asked (default).
3. Main store listing: paste text from 1-store-listing.md, upload graphics/, add the Nepali translation.
4. App content: complete every form as in 2-play-console-answers.md. The SMS declaration needs the demo video.
5. Testing > Internal testing: create a release, upload the .aab, add yourself as a tester, install from the Play link and repeat the quick checks (setup, permission, a real payment SMS, voice). This is also the first install through Play, so Play Protect should treat it like a normal Play app (not verified here).

## Step 4. Closed test, then production
1. Testing > Closed testing: same bundle, add the 12+ testers, keep them opted in 14 days.
2. When eligible, Dashboard > Apply for production access, answer the questions honestly (what testers did, what you changed).
3. Create the production release and send for review. Google reviews the SMS declaration as well, which can take longer than a normal review. Approval is not guaranteed; if they ask for changes, send me their message and we adjust.

## Later updates
Each production build gets the next version code automatically. Upload the new .aab to a release. Keep the privacy policy, Data safety and SMS declaration in step with any change (for example, never add analytics or network code without updating all three).

## If Google rejects the SMS permission
Typical reasons are an unclear core purpose, a disclosure that does not match the text in the app, or a demo video that does not show the permission flow. Fix those first. A fallback to discuss would be reading payment alerts from notifications instead of SMS, which has its own permission and disclosure rules and has not been looked at in this project.
