## 0.4.0-preview.1 — Nonblocking insufficient data

Insufficient model data now returns a structured developer result from handoff, training and result APIs. Polling stops without a warning screen; authentication and authorization failures still throw. The native flow remains host-controlled.

Validated by private source CI at commit `7f9a52678d3ef6ab5be573169b24632690ed5b32`: 33 Kotlin tests, 5 shared-script tests, 9 Android runtime tests, Android builds and lint. No physical-device/provider acceptance was run. Maven Central publication remains pending its existing account setup; this preview is available through the GitHub binary repository.

# Changelog

## 0.3.0-preview.2

Adds rotating owner sessions with lost-response recovery, PIN setup/unlock grants, guided native onboarding using RN 4.6 typography, account-state cleanup, non-blocking import teardown and runtime/layout regression tests. Backend fixes are delivered separately on a dev feature branch. Real-device/provider and exact visual parity remain acceptance gates.

## 0.3.0-preview.1

Adds device approval/PKCE and partner exchange integration, TikTok browser and official Android authorization, hosted/private native imports, receipt-bearing raw exports, temporary files, updated demo and source-free release tooling. Backend/provider/device acceptance gates are documented; this is a preview, not full live parity certification.
