# STRICT AUDIT — 207200 Chromium 153 Android

## Scope
This document defines the release-gate audit for the Android ARM64 build.

## 1. DRM / Widevine
- [ ] Chromium 153 source revision matches the intended build.
- [ ] `enable_widevine=true`.
- [ ] No Cromite patch disables Android MediaDrm.
- [ ] Widevine software-secure registration remains present.
- [ ] Widevine hardware-secure registration remains present.
- [ ] MediaDrm Widevine UUID support succeeds on the test device.
- [ ] MediaDrm provisioning succeeds.
- [ ] Security origin handling succeeds.
- [ ] MediaCrypto creation succeeds.
- [ ] EME key-system discovery succeeds.
- [ ] License/key-session exchange succeeds.
- [ ] L3 / SW_SECURE_CRYPTO playback succeeds where supported.
- [ ] L1 / HW_SECURE_ALL playback succeeds where supported.
- [ ] HDCP behavior matches the device/security level.
- [ ] Encrypted H.264 playback tested.
- [ ] Encrypted VP9 playback tested where available.
- [ ] Encrypted HEVC playback tested where available.
- [ ] Playback survives navigation, pause/resume, reload, and browser restart.

## 2. Course-site regression
- [ ] bassthalk course page loads.
- [ ] Protected video stays in browser when the site supplies browser EME.
- [ ] If the site intentionally requests an external player, the Android intent is dispatched normally.
- [ ] No silent arbitrary external-app launch.
- [ ] User-gesture protections remain intact.
- [ ] Locked-device intent protection remains intact.

## 3. Media / codecs
- [ ] H.264 + AAC.
- [ ] HEVC.
- [ ] AV1.
- [ ] Hardware decoder selection.
- [ ] Audio focus/background audio.
- [ ] Seek and variable playback rate.
- [ ] Fullscreen and orientation changes.
- [ ] HDR behavior checked; current Cromite patch set explicitly disables Android HDR, so this must be documented as an intentional limitation or reconsidered.

## 4. Browser security
- [ ] HTTPS/TLS.
- [ ] Certificate validation.
- [ ] Safe Browsing/link protection.
- [ ] sandbox/process isolation.
- [ ] site isolation.
- [ ] mixed/cleartext HTTP policy.
- [ ] malicious-download protections.
- [ ] popup/navigation protections.
- [ ] external-intent abuse protections.
- [ ] locked-device intent protections.

## 5. Privacy
- [ ] ad/tracker blocking.
- [ ] third-party storage partitioning.
- [ ] cookie partitioning.
- [ ] WebRTC local-IP protection.
- [ ] fingerprinting mitigations.
- [ ] Do-Not-Track / GPC behavior.
- [ ] telemetry/crash-reporting configuration.
- [ ] DNS/DoH behavior.
- [ ] permissions and site settings.

## 6. Ad blocker
- [ ] filter engine initializes.
- [ ] network blocking works.
- [ ] cosmetic/subresource rules work.
- [ ] allowlist works.
- [ ] per-site controls work.
- [ ] no crash when filter lists update.
- [ ] no accidental blocking of DRM/license requests.
- [ ] no accidental blocking of course-video CDN requests.

## 7. Extensions / userscripts
- [ ] Android extension support builds.
- [ ] extension install flow works.
- [ ] content scripts execute.
- [ ] permissions are enforced.
- [ ] incognito behavior matches settings.
- [ ] userscript support does not break page isolation/security.

## 8. DevTools
- [ ] Android DevTools frontend loads.
- [ ] inspect fallback works.
- [ ] console/network/sources panels work.
- [ ] remote/custom protocol exposure remains disabled unless explicitly intended.
- [ ] no unintended debugging endpoint is exposed.

## 9. Performance / stability
- [ ] cold start.
- [ ] warm start.
- [ ] tab creation/destruction.
- [ ] long video playback.
- [ ] background/foreground.
- [ ] memory pressure.
- [ ] low-memory Android behavior.
- [ ] GPU process crashes.
- [ ] renderer crashes.
- [ ] browser process crashes.
- [ ] ANR checks.
- [ ] battery/thermal behavior.

## 10. Build integrity
- [ ] ARM64 APK produced.
- [ ] package name is correct.
- [ ] version metadata is correct.
- [ ] release signing works.
- [ ] APK installs/upgrades/uninstalls cleanly.
- [ ] native libraries are ARM64 and load correctly.
- [ ] no missing resources/assets.
- [ ] no unresolved Chromium patch conflicts.
- [ ] no stale Chromium-version patches remain without review.
- [ ] temporary/WIP patches are explicitly reviewed.

## 11. Mandatory release rule
A green build is NOT sufficient. The release is considered passing only after source-level patch application succeeds AND the runtime Android test matrix passes. Any failed DRM, security, crash, or intent test blocks release until investigated.

## Current known limitation
The 207200 repository is a Chromium patch/build layer rather than a complete Chromium source checkout. Therefore this checklist cannot honestly be marked fully passed until the exact Chromium 153 source is available to the build/test environment and an APK is executed on an Android device.
