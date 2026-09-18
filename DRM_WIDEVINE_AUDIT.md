# DRM / Widevine / MediaDrm Audit — Chromium 153 target

## Current branch
- Branch: `drm-widevine-v153`
- Repository: `2ed-code/207200`
- Target: Android ARM64 Chromium/Cromite-derived browser

## Changes already applied
1. Removed `Disable-DRM-media-origin-IDs-preprovisioning.patch` from the patch list.
   - This stops Cromite from explicitly disabling Chromium's MediaDrm preprovisioning feature.
2. Added `enable_widevine = true` to `build/cromite.gn_args`.
   - On Android this selects Chromium's MediaDrm integration; it does not bundle a separate Widevine CDM.
3. Removed the old `Add-flag-to-disable-external-intent-requests.patch` from the patch list.
   - This avoids carrying an old version-sensitive intent patch into Chromium 153.
   - Chromium's normal user-gesture/security handling remains in force.
4. Kept `Block-Intents-While-Locked.patch`.

## Chromium reference checks
Chromium's Android implementation registers Widevine through Android MediaDrm and checks protected-content support at playback time. The MediaDrm bridge creates Android `MediaDrm`, handles provisioning, key sessions, MediaCrypto, security level and HDCP state.

## Patch-list audit
The current patch list was reviewed for obvious DRM/Widevine/MediaDrm disabling patches. No additional patch name explicitly disables Widevine or Android MediaDrm.

The following media-related configuration remains intentionally enabled:
- proprietary codecs
- platform AAC
- platform H.264
- platform HEVC
- AV1/dav1d

## Important remaining validation
This repository is a patch/build layer and does not contain the complete Chromium source tree. Therefore a true source-level compile validation and runtime DRM test cannot be claimed from this repository alone.

Before calling DRM complete, the Chromium 153 source must be checked against the patch set and the resulting APK must be tested on a real Android device for:
- `MediaDrm.isCryptoSchemeSupported(Widevine UUID)`
- Widevine provisioning
- EME key-system discovery
- streaming license/key request
- MediaCrypto creation
- software-secure playback
- hardware-secure playback where the device supports it
- HDCP behavior
- protected H.264/VP9/HEVC playback
- repeated playback after browser restart
- origin/storage handling for DRM sessions

## External-app intent validation
The old custom intent patch was deliberately not reused because it is version-sensitive. A Chromium-153-compatible implementation should be added only after the current Android intent flow is inspected. The desired design is:
- explicit user-controlled Allow external apps setting
- enabled by default
- user-gesture protection retained
- locked-device protection retained
- no silent/background arbitrary app launches
- tel/mailto and other normal intents continue through Chromium's normal policy

## Release gate
Do not label an APK as DRM-working until both build-time and runtime checks pass. A successful Chromium build by itself is not proof that Widevine playback works.
