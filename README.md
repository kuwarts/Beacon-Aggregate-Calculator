# Aggregate Quote

A fast quote calculator for aggregate deliveries, styled like the iPhone calculator. Pick the aggregate and truck, type the volume and distance, and the price appears. Tap **Book** to save the booking with the date and time.

GitHub builds the apps for you. There are three ways to install, and you can use more than one.

| | iPhone | Android | Cost |
|---|---|---|---|
| **Web app** (GitHub Pages) | ✓ | ✓ | Free |
| **Android app** (.apk) | | ✓ | Free |
| **App Store / TestFlight** | ✓ | | Apple Developer Program, $99/year |

---

## 1. Put this folder on GitHub

1. Go to **github.com/new**. Name the repository `aggregate-quote` and choose **Public**. GitHub Pages is free for public repositories; a private one needs a paid GitHub plan. Click **Create repository**.
2. Upload everything in this folder, **including the hidden `.github` folder**. It holds the build instructions.
   - The easiest way is **GitHub Desktop**: *File › Add local repository*, pick this folder, then *Publish*.
   - If you drag files into the browser instead, show hidden files first (Mac Finder: `Cmd + Shift + .`) so `.github` gets uploaded too.

## 2. Turn on the web app

1. In the repository, open **Settings › Pages**.
2. Under *Build and deployment › Source*, choose **GitHub Actions**.
3. Open the **Actions** tab and wait for **Web app** to turn green (about a minute). If it ran before Pages was switched on, click it and choose **Re-run all jobs**.
4. Your app is now at `https://<your-username>.github.io/aggregate-quote/`.

**Install on iPhone:** open that link in **Safari** › Share button › **Add to Home Screen**.
**Install on Android:** open it in **Chrome** › ⋮ menu › **Add to Home screen** (or **Install app**).

It opens full screen with no browser bar, and it works offline after the first open.

## 3. Android app (.apk)

The **Android app** workflow builds an installable app every time you push to `main`.

- **From a computer:** Actions tab › latest **Android app** run › *Artifacts* › `AggregateQuote-android` (a zip containing `AggregateQuote.apk`).
- **Straight to the phone (easier):** on GitHub open **Releases › Draft a new release**, type the tag `v1.0.0`, and publish. When the build finishes, `AggregateQuote.apk` is attached to that release. Open the release on your Android phone and tap the file.

Android will ask you to allow installing apps from this source the first time.

**Google Play (optional):** add these repository secrets (*Settings › Secrets and variables › Actions*) and the workflow also produces a signed `.aab` ready for the Play Console:
`ANDROID_KEYSTORE_BASE64`, `ANDROID_KEYSTORE_PASSWORD`, `ANDROID_KEY_ALIAS`, `ANDROID_KEY_PASSWORD`.
Create a keystore once with
`keytool -genkey -v -keystore release.keystore -alias quote -keyalg RSA -keysize 2048 -validity 10000`
and store the output of `base64 -i release.keystore` as the first secret. Keep that keystore safe: Play needs the same one for every update.

## 4. iPhone app (App Store / TestFlight, optional)

Apple only lets apps from the App Store or TestFlight onto an iPhone, so the web app from step 2 is the free route. If you join the Apple Developer Program:

1. In App Store Connect create an app with bundle ID `com.aggregatequote.app`.
2. Add these repository secrets: `IOS_CERTIFICATE_BASE64`, `IOS_CERTIFICATE_PASSWORD`, `IOS_PROFILE_BASE64`, `IOS_PROFILE_NAME`, `IOS_TEAM_ID`, `ASC_KEY_ID`, `ASC_ISSUER_ID`, `ASC_KEY_BASE64`.
3. Push a tag such as `v1.0.1`. The **iPhone app** workflow builds the app on a Mac and uploads it to TestFlight.

Without those secrets, the iPhone workflow still runs and checks that the app compiles. Start it from the Actions tab.

---

## Your data

Rates and bookings are saved on each phone. They don't sync between phones.

- **Back up:** Rates › **Copy backup**, then paste it into Notes or a message to yourself. **Restore from a backup** brings it back, on the same phone or a new one.
- **Google Sheets:** Bookings › **Copy all for Google Sheets**, then paste into cell B5 of the Bookings tab in the spreadsheet version.

## Making changes

- The whole app is one file: `www/index.html`. Edit it, push, and all three builds run again.
- After changing the web app, raise `VERSION` in `www/sw.js` (for example `aq-v2`) so installed copies pick up the update.
- Raise `"version"` in `package.json` for each Android release; the build number goes up on its own.
- The app's name and ID live in `capacitor.config.json`. Change the ID **before** your first store upload; it can't be changed afterwards.
- To change the icon, replace the images in `assets/` (and `www/icons/` for the web app).
