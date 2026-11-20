# Sologuti Android release preparation

## App identity

- App name: Sologuti – ১৬ গুটি
- Android application ID: `com.sajednsu.sologuti`
- Website: https://sajednsu-sketch.github.io/sologuti/
- Distribution target: Google Play
- Package approach: Trusted Web Activity (TWA) using Bubblewrap
- First Android version: 1 (version code 1)
- Target SDK: Android 16 / API 36 or later for a new Google Play submission as of 31 August 2026.

## Current status

- [x] Existing game runs as a PWA.
- [x] App manifest has a stable `id`.
- [x] Privacy policy is published.
- [ ] Confirm production website origin and Digital Asset Links hosting.
- [ ] Generate Android signing key and TWA project.
- [ ] Add the signing certificate SHA-256 fingerprint to Digital Asset Links.
- [ ] Build and test an Android App Bundle (.aab).
- [ ] Complete Play Console account verification and app listing.
- [ ] Complete Play testing and policy declarations.
- [ ] Submit for review.

## Important hosting requirement

A Trusted Web Activity needs a Digital Asset Links file at the **origin root**:

`https://YOUR-DOMAIN/.well-known/assetlinks.json`

The current project is hosted at a GitHub Pages project path (`/sologuti/`). GitHub Pages project sites cannot publish that file at the root of the shared `github.io` origin for this app. Before generating the final TWA, choose a domain/origin where the root `.well-known/assetlinks.json` can be served. A custom domain pointed to this GitHub Pages site is one practical option.

Do not publish a placeholder assetlinks file. It must contain the exact SHA-256 fingerprint of the app's signing certificate. If Google Play App Signing is enabled, the Play app-signing certificate fingerprint must also be included for the installed Play version to verify as a TWA.

## Build environment

The project can be prepared on Windows. Bubblewrap requires Node.js and an Android build environment; its setup wizard can install the Android dependencies. Keep the signing keystore and passwords private and backed up. Never commit them to GitHub.

## Suggested first build commands

After the production origin has been confirmed:

```powershell
npm install -g @bubblewrap/cli
bubblewrap init --manifest=https://YOUR-DOMAIN/manifest.webmanifest
bubblewrap build
```

Review the generated configuration before building. Confirm the application ID is exactly `com.sajednsu.sologuti` and target SDK is at least 36. The final Play upload should be an Android App Bundle (.aab).

## Store listing direction

- Title: Sologuti – ১৬ গুটি
- Short description: Play the traditional Bangladeshi 16-piece strategy board game.
- Category: Games / Board
- Pricing plan: Free initially unless a separate monetisation decision is made.
- Privacy policy: https://sajednsu-sketch.github.io/sologuti/privacy.html

Do not claim multiplayer over the internet: the current game supports local two-player mode and play against AI.


## Automated build workflow

The GitHub Actions workflow generates the Trusted Web Activity project directly from `twa-manifest.json` using Bubblewrap's non-interactive `update` command, then builds an unsigned test package. A signed Play Store release still requires the preserved private signing key.
