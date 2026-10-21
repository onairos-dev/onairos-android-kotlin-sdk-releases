# Install and test the Android Kotlin preview

Version: `0.3.0-preview.1`. Implementation source is private. Download the demo APK and Maven binary bundle from [GitHub Releases](https://github.com/onairos-dev/onairos-android-kotlin-sdk-releases/releases).

The core library is `com.onairos:onairos-kotlin`; the Android library is `com.onairos:onairos-android`. The Android library includes the core transitively. Use Java/Kotlin target 17 and Android API 28+ for the private import UI.

```kotlin
repositories {
    google()
    mavenCentral()
    maven("https://artifact.bytedance.com/repository/AwemeOpenSDK") {
        content { includeGroup("com.tiktok.open.sdk") }
    }
    maven("https://raw.githubusercontent.com/onairos-dev/onairos-android-kotlin-sdk-releases/dev/maven") {
        content { includeGroup("com.onairos") }
    }
}
dependencies {
    implementation("com.onairos:onairos-android:0.3.0-preview.1")
}
```

These preview coordinates are hosted in the public GitHub Maven directory, not Maven Central. The downloadable Maven ZIP can also be extracted and used with a local `maven { url = uri("/path/to/maven") }` repository. Include transitive dependencies through Gradle; the standalone JAR/AAR is not a fat library.

## Demo registration

1. Install the APK on an Android 9+ Google Play-enabled device with an updated Android System WebView and a browser. Use a test account.
2. Register package `com.onairos.demo` and the debug certificate in `verification.json` in the same Google Cloud project as the server client ID. The Android client registration authorizes the APK; Credential Manager requests an ID token for the **Web/server client ID** accepted by the backend.
3. Enter the API HTTPS origin, mobile developer API key, registered Onairos application ID and Google server client ID. No real credentials are embedded in the APK.
4. Sign in with Google, or request and verify an email code. Load connectors. Existing OAuth and API-token connections can be tested with the corresponding provider configuration. Connection state is checked with the server.
5. Select the processing/sharing consent checkbox. Run training. The demo obtains a mobile handoff, starts/reuses a durable job when needed, polls the supplied status URL and fetches the returned result URL when ready. Stop waiting cancels the local wait, not an already-admitted server job.

## Device approval

Install the Onairos app and register `onairos-kotlin-demo://approval/callback` for this application. Configure your own authenticated HTTPS partner exchange endpoint in the demo. That endpoint must exchange the code with Onairos using the developer/client secret **on the server only**, forwarding `code`, `client_id`, `redirect_uri`, `code_verifier`, `state` and `grant_type`.

Tap the device approval button. The SDK opens `onairos://authenticate` with S256 PKCE and requested permissions. It verifies exact callback location, fixed query parameters, state, code shape, expiry and one-shot use before invoking your partner exchange. The exchange response must return the matching state, scoped token and `authorizedData`. Approved public profile, traits and personalization are fetched independently with the exchanged grant token; the owner's signed-in token is never substituted. Missing inference items produce an incomplete/partial result rather than exposing unapproved data.

The matching Onairos backend must enforce PKCE, app/redirect binding, code expiry and single use, and return the current grant response. An older cross-app token route is insufficient. A private team partner-exchange example is included in the development repo. This is an explicit host/backend integration requirement, as in React Native 4.6.0.

## TikTok

**System browser:** use the TikTok web-login button. It uses the backend's existing web registration and start-ticket route, binds the return link to a random attempt and the current user, and checks server connection status after return.

**Official native:** the Android library includes TikTok Login Kit. Configure Android TikTok client credentials, approved scopes and signing certificates server-side. Deploy `/connectors/native/tiktok/config`, `/start`, and `/complete`. Forward native activity results and warm callback intents to `TikTokOfficialAdapter.onCallback`. For browser fallback, the app must own the backend-configured HTTPS App Link and have a matching manifest/Digital Asset Links association; the generic supplied APK uses a placeholder host. Team builds can set `nativeRedirectHost` and `nativeRedirectPath` Gradle properties.

TikTok registration uses the APK signing certificate's MD5/SHA-256 values; Google registration uses its SHA-1. All are listed in `verification.json`. The signing certificate SHA-256 is different from the APK file checksum in `SHA256SUMS.txt`.

Discord uses the same host-owned official SDK bridge interface as the reference SDK. Register a `NativeConnectorAdapter` wrapping the official Discord SDK with `NativeConnectorRegistry`; the demo does not fabricate a configured bridge. Missing native backend routes or provider registration must show unavailable/failure, not connected.

## Hosted and native imports

Choose a provider from the catalog, then the hosted or native-browser button. Hosted ChatGPT, Claude, Gemini, Grok and Instagram imports use the server's disposable Chromium viewer, queue and topic-approval UI. All per-session control tokens stay private and are not logged.

Native ChatGPT/Claude imports use the shared React Native 4.6.0 extraction and message-validation algorithms in a separate private WebView process. Sign in with the provider, list titles, choose topics, review individual chats and import. Sensitive topics start off; exclusions apply; selecting nothing imports nothing. Normal mode saves selected conversations. Ephemeral mode returns only server-redacted, receipt-bearing data through an expiring one-shot in-memory result pipe. No unredacted fallback is returned if privacy processing fails.

Files require separate opt-in and native-files backend support. They stream through bounded acknowledged messages to temporary authorized delivery. The demo can list files and open Android's document picker to save a chosen file. Downloads use file capabilities, never the owner's JWT, and enforce origin/path/expiry constraints and full-download SHA-256 when supplied.

Instagram, Spotify, Netflix, Sephora, Telegram and Hinge use the shared reference extraction/payload-normalization scripts and authenticated backend storage. LinkedIn discovers the profile URL and uses the reference backend scrape path. These website-dependent paths are not live-certified by a build; the inherited Hinge web path is a placeholder in the reference release.

## Acceptance status

The preview is built and protocol-tested, but no real-device/provider matrix was executed during preparation. The backend checkout used for development lacks the newer native-provider routes and the 4.6 device-approval PKCE/grant contract. Deploy the matching backend before claiming end-to-end support. Provider credentials, native bridge registration, API entitlement, website availability, worker health and consent remain server/provider-controlled.

The preview does not claim pixel-identical React Native UI or a newly certified production PIN/proof UI. It does not implement the reference's mock `encryptModelKey` string as real encryption. Keep source and signing keys private. Contact Onairos for production licensing terms.
