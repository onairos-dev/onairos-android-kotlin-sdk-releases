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

## Maven Central: step by step

1. **Choose final coordinates and distribution terms.** Current preview coordinates are `com.onairos:onairos-kotlin` and `com.onairos:onairos-android`. Agree the SDK's proprietary/evaluation/production license terms. No open-source license is implied by public binaries.
2. **Create a Central Portal publisher account** at [central.sonatype.com](https://central.sonatype.com/).
3. **Verify a namespace you control.** `com.onairos` requires ownership of the corresponding domain; `onairos.uk` would normally correspond to `uk.onairos`. Choose and verify the namespace deliberately before the first Central release, and update group IDs/consumer instructions if needed. Follow [namespace registration](https://central.sonatype.org/register/namespace/).
4. **Complete publication metadata:** real project URL, description, license, developer/organization and SCM information. SCM may point at a private repository; source access need not be public. Follow [Central requirements](https://central.sonatype.org/publish/requirements/).
5. **Prepare documentation and sources-classified artifacts without leaking implementation.** For closed-source publication, Sonatype explicitly permits a sources JAR containing a notice/README instead of implementation source. Supply API documentation/Javadoc artifacts too. Do not upload the private build's real `-sources.jar` by accident. See [closed-source publishing FAQ](https://central.sonatype.org/faq/closed-source/).
6. **Create a publisher token and GPG/PGP signing key.** Store publisher credentials and private signing material only in private CI secrets or your local secret store. Generate signatures and the required checksums for each publication artifact.
7. **Configure a Central Portal-compatible Gradle publication/deployment workflow.** The existing `publish` tasks only write local staging repositories; they are not an automatic Central deployment. Follow [Sonatype's Gradle guidance](https://central.sonatype.org/publish/publish-portal-gradle/) or upload the Maven-layout bundle using the [Portal API](https://central.sonatype.org/publish/publish-portal-api/).
8. **Run the full release gates:** protocol tests, script tests, Android lint, APK/AAR assembly, source/secret audit and the real-device/provider matrix. Confirm supported backend versions and registrations. Build a new immutable release version.
9. **Upload, validate and explicitly publish** in the Central Portal. Resolve validation errors before publishing. Review the current [publisher terms](https://central.sonatype.org/publish/producer-terms/) for this service-dependent SDK.
10. **Verify from a clean consumer app.** Replace the GitHub Maven URL with `mavenCentral()` plus `google()` and the provider's required repository, then resolve the final coordinates and run a release build. Share the dependency snippet and migration notes. Keep implementation source in the private repo.

The GitHub release setup does not verify a Central namespace, choose a legal license, install signing keys, or create a Central publisher account for you. Those remain owner-controlled prerequisites. No Maven Central upload has been performed by this setup.
