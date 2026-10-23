# Onairos Android Kotlin SDK — binary releases

Implementation source stays private. This repository contains public binaries, Maven metadata and integration documentation only.

Latest preview: **0.4.0-preview.3**. [Download the APK, AAR, core JAR and Maven bundle](https://github.com/onairos-dev/onairos-android-kotlin-sdk-releases/releases/tag/v0.4.0-preview.3).

[Android setup and demo testing](docs/ANDROID_INTEGRATION.md) · [Public distribution steps](docs/DISTRIBUTION.md)

This is a preview, not a certification of complete React Native feature parity. The named device approval, TikTok, hosted/native import, file-delivery and training paths are implemented; live provider/device/backend acceptance remains required. Discord's official bridge is host-owned, and inherited placeholder web providers are not certified.

No API keys, user tokens or signing keys are included. APK signing fingerprints are in `verification.json`. Register the package/certificate and configure your app before testing. Contact Onairos for production licensing terms; no open-source license is granted for the private implementation.

Current coordinates: `io.onairos:onairos-android:0.4.0-preview.3` and matching `onairos-kotlin`. Version 0.4.0-preview.3 is published on Maven Central. A clean Gradle consumer resolved both modules and their dependencies, and SDK binaries matched the signed upload. The version-pinned GitHub Maven repository remains an optional backup. [Android guide and setup skill](https://docs.onairos.io/kotlin-integration/) · [Central setup and repeat release process](docs/MAVEN_CENTRAL.md).

Preview.3 adds the guided connector UI, provider-specific routing and title-first ChatGPT/Claude Smart Filter. Paused/unsupported providers, including Instagram and Telegram, remain hidden in the guided catalog. See the setup guide and release notes for backend/provider prerequisites and remaining acceptance limits.
