# Preview 2 verification and device acceptance

Reference: React Native 4.6.0 / e612ce2. SDK binaries: 0.3.0-preview.2. Source is private.

| Verification | Result | Limit |
| --- | --- | --- |
| Backend approval, native OAuth, PIN/setup races, browser/app binding, handoff/revoked JWT | 167 tests pass across 21 files | Synthetic persistence/provider mocks; backend PR 726 is not deployed |
| Kotlin protocol/session tests | 30 pass | Mock HTTP, not live providers |
| Android lifecycle/layout tests | 9 pass, APIs 28 and 35 | Robolectric runtime; includes import recreation, demo recreation/callbacks, large fonts and synthetic render generation |
| Shared RN connector algorithms | 5 pass | Selection, sensitive topics, message validation, normalizers and Netflix preservation |
| Android SDK/demo | APK, AAR and instrumentation APK compile; lint 0 errors | Physical instrumentation/Keystore tests not run |
| Public distribution | Anonymous Maven JAR download and release SHA-256 digests verified; separate app builds from public binaries | No private composite source used in the consumer build |

[UI renders](screenshots/) are synthetic Android renders for visual review. The guided UI uses the RN fonts/tokens but is not pixel-identical: provider/hosted screens are externally controlled; native topic/chat selection uses dialogs; exact RN animations/avatar/presentation overrides and passkey management are not duplicated. Discord needs a host official adapter; Hinge inherits an unsupported placeholder. Apple needs a host identity-token integration. Hardware biometric custody needs real-device validation.

## Real-device setup

1. Connect Android 9+ by USB; enable Developer options/USB debugging; unlock and accept this computer's prompt. Update Chrome and Android System WebView. Tell the tester model/OS version. A Google Play-enabled device is needed for Google Credential Manager testing.
2. Install the release demo APK. Register package com.onairos.demo and the actual SHA-1/MD5/SHA-256 certificate fingerprints in verification.json with the provider registrations. Use the backend-accepted Web/server Google client ID, not an Android client ID in that field.
3. Use a non-production API with backend PR https://github.com/zd819/TempBackend/pull/726 deployed and compatible training/hosted/privacy workers. Configure the mobile developer key, application ID and provider registrations locally; do not send secrets in chat or logs.
4. Native TikTok needs a registered Android client, test-user access/scopes and the configured HTTPS App Link. Rebuild the APK with the matching nativeRedirectHost/nativeRedirectPath properties and publish Digital Asset Links. Install TikTok. Discord requires the host official bridge.
5. For device approval install the Onairos app, register the partner callback and configure an authenticated partner HTTPS exchange endpoint. Server/client secrets stay on that backend.
6. Use synthetic accounts with representative permitted data, a returning trained account and a new/untrained account. Enroll a strong biometric if testing device PIN custody.

## Required live matrix (not yet executed)

- Login: owner Google/email and standard app login; wrong/expired code; denied credentials; network failure; new/returning account; logout/relogin and account switch.
- Session: expiry, refresh concurrency, lost response/restart within grace, revoked session, offline/server outage, pending rotation sign-out and password/PIN security changes. Confirm no fallback to a cached owner/grant token.
- Approval: approve/deny, malformed/duplicate/expired/wrong-state links, replay, cold/warm return, wrong app/redirect, partner exchange failure, partial approved data and insufficient preference inputs.
- Connectors: native TikTok installed/uninstalled/cancelled; browser callbacks; Discord bridge; hosted topic approval; each supported native website using current WebView. Connection flags must be server-confirmed. Identity-only links must not count as training sources.
- Imports/privacy: empty selection, sensitive topics off, exclusions and chat review; cancellation/back; orientation and OS process kill; cache/cookie isolation between accounts and host WebViews; receipt failure; files opt-in, chunk failure, manifest expiry/path rejection and download checksum mismatch.
- PIN/biometrics: new setup vs existing-code unlock; wrong code/lockout; server vs device custody; prompt success/cancel/lockout; enrollment changes, invalidated key, reinstall/restore, account switch and manual fallback. Verify no plaintext PIN persists.
- Training: fast/full/background paths; worker/API compatibility; dedupe; queued/running/ready/failed/insufficient-data; timeout/resume; revoked consent and explicit restoration; returned traits/inference endpoint.
- UI: portrait/landscape, keyboard, background/resume, small screen/tablet, large font, TalkBack, system bars, disabled/busy/error states, provider return and comparison against RN 4.6 reference screenshots.

Passing these checks reduces risk; no test suite proves the absence of all runtime defects. Record devices, backend/SDK commits, provider registrations and observed results before stable parity sign-off.

Independent Linux CI passed on private source commit `8a2cd22`: https://github.com/onairos-dev/onairos-android-kotlin-sdk/actions/runs/39586597739. The workflow builds/tests the same SDK implementation, runs lint and prepares source-free artifacts. Public release binaries retain their original build/checksums and source commit in verification.json.
