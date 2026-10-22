# Public distribution, with implementation source private

There are two repositories:

- **Private development:** `onairos-dev/onairos-android-kotlin-sdk` — source, demo, tests, shared private RN scripts and build automation.
- **Public release:** `onairos-dev/onairos-android-kotlin-sdk-releases` — compiled AAR/JAR/APK, Maven metadata, checksums, docs and release notes.

Kotlin/Java binaries and client-side runtime assets can be inspected or decompiled; keeping the source repo private is not a promise that shipped client code is secret. Never embed server secrets in an SDK or APK.

## GitHub downloads: available first

1. Build and test in the private repo. Run the shared-script tests and Gradle SDK tests, build the Android AAR/demo APK and run lint.
2. Run the release preparation script. It copies implementation files only to private staging and constructs a separate public Maven tree. Source JARs/documentation variants referencing actual source are removed. Public archives are inspected recursively for implementation source and signing material.
3. Review the preview/release status, compatibility matrix, debug fingerprint and SHA-256 checksums. Do not label mock/compiled tests as live provider certification.
4. Commit source changes to private `dev`. Commit only the prepared binary Maven tree and public docs to public `dev`.
5. Create a versioned GitHub Release in the public repo. Upload the AAR, core JAR, demo APK, Maven ZIP, verification metadata and checksums. Use preview tags until the real-device acceptance matrix passes. Reuse neither a released version nor its binary content for a different build.
6. Share the public release URL. Consumers can install the APK or use the GitHub-hosted Maven repository shown in the Android integration guide. No private repository access is needed to consume public binaries.

The private CI builds and checks source. Public-release publishing requires a narrowly scoped `PUBLIC_RELEASE_TOKEN` with write access to the public release repository; do not grant or expose private source access through the public repo. Store it as a private repository Actions secret. A normal private `GITHUB_TOKEN` cannot generally publish to another repository.

## Maven Central

The current group is **io.onairos**, verified through onairos.io. The documented preview is **0.4.0-preview.2**. Earlier com.onairos preview coordinates remain immutable. Central and GPG publishing credentials are configured in the private source repository's Actions secrets; SDK implementation source remains private.

Version **0.4.0-preview.2 is PUBLISHED on Maven Central**, verified on 2026-10-21. A clean Gradle consumer resolved Android/core and transitive dependencies using Central for io.onairos, with no GitHub fallback; the SDK binaries matched the signed upload. The pinned GitHub Maven repository remains an optional backup. Each future version requires its own signed upload, final publication and consumer verification.

Use [MAVEN_CENTRAL.md](MAVEN_CENTRAL.md) for namespace/signing details, the GitHub workflow, final Portal publication and the repeatable process for every SDK update. Every update requires a new version, tests, binary/source audit, signed upload, explicit publication, public release and matching docs/skill metadata. A build or upload is not evidence of provider/device acceptance.

## Developer links

- Setup and package instructions: https://docs.onairos.io/kotlin-integration/
- AI setup skill ZIP: https://docs.onairos.io/skills/onairos-kotlin.zip
- AI guide: https://docs.onairos.io/kotlin/llm.txt
- Release downloads: https://github.com/onairos-dev/onairos-android-kotlin-sdk-releases/releases/tag/v0.4.0-preview.2

Maven coordinates: `io.onairos:onairos-android:0.4.0-preview.2`, with the matching core included transitively. Use Gradle repository metadata rather than adding a standalone AAR without its dependencies.
