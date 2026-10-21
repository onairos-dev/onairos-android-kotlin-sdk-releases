# io.onairos Maven Central publishing

The next candidate is `0.3.0-preview.3`, under `io.onairos:onairos-kotlin` and `io.onairos:onairos-android`. Kotlin package names remain `com.onairos.sdk`; existing `com.onairos` preview 2 binaries remain immutable. This candidate is not yet on Central and does not imply backend/provider/device acceptance.

## One-time account setup

1. Sign in at https://central.sonatype.com and add namespace `io.onairos`.
2. Copy its public Verification Key. Add a separate root TXT record on `onairos.io`; retain existing SPF, DKIM, DMARC and other TXT records. Wait until the TXT record resolves before asking Sonatype to verify it.
3. With a locally configured Cloudflare token restricted to Zone Read/DNS Edit for this zone, preview with `python scripts/central-dns.py --key VERIFICATION_KEY`. Add it with `--apply`, then check propagation with `--check`. The helper never updates or deletes existing records. Alternatively use Cloudflare's DNS dashboard.
4. Generate a Central user token at https://central.sonatype.com/usertoken. Store its username/password only as private source-repository Actions secrets `MAVEN_CENTRAL_USERNAME` and `MAVEN_CENTRAL_PASSWORD`.
5. Use a dedicated GPG signing key. Publish its public key to a public keyserver supported by Central; keep its secret key and passphrase private. Add private repository secrets `MAVEN_SIGNING_KEY` (armored private key), `MAVEN_SIGNING_FINGERPRINT` (full fingerprint), and `MAVEN_SIGNING_PASSPHRASE`. Never put these secrets in the public release repository or upload artifacts.
6. Review the proprietary licensing notice and Sonatype publisher terms before first publication. This setup supplies a contact/rights notice, not a new SDK license agreement. Use the approved final licensing URL in both module POMs if different.

## Build and upload

Run **Maven Central preparation and validation** in the private repository's Actions tab. `prepare` runs shared-script tests, Kotlin and Android tests, lint, builds, publication/source checks and creates an unsigned review bundle. `upload-for-validation` also imports the configured signing key, signs each publication file, verifies signatures locally and uploads a signed bundle using `USER_MANAGED` mode.

The bundle uses Maven root layout (`io/onairos/...`), contains only the selected two modules/version, includes complete POM metadata and checksums, and substitutes notice-only sources artifacts. Documentation JARs contain an HTML integration index and the public setup guide; they are not a complete generated API reference. Actual implementation source is never attached. The ordinary GitHub binary bundle and this Central bundle are distinct artifacts.

The workflow does not automatically publish. After Portal validation, inspect its results and publish the candidate deliberately. Stable publication still requires the backend rollout and physical-device/provider/UI acceptance documented in TESTING.md. An uploaded/validated preview does not satisfy that gate. Central release versions cannot be overwritten.

For a local build, run the normal SDK build/publish tasks, then:

```sh
python -m unittest discover -s scripts -p 'test_central*.py'
python scripts/central-publish.py prepare
# With a configured/imported private signing key and environment secrets:
python scripts/central-publish.py prepare --sign --output output/central-0.3.0-preview.3-signed.zip
python scripts/central-publish.py upload --bundle output/central-0.3.0-preview.3-signed.zip
python scripts/central-publish.py status --deployment-id DEPLOYMENT_ID
```

Never blindly retry an upload after an uncertain network outcome. Check Portal deployments first. The helper reports the deployment ID and leaves final publication to the account owner.

After publication, verify a clean consumer app resolves:

```kotlin
repositories {
    google()
    mavenCentral()
    maven("https://artifact.bytedance.com/repository/AwemeOpenSDK")
}
dependencies {
    implementation("io.onairos:onairos-android:0.3.0-preview.3")
}
```

Official guidance: [namespace verification](https://central.sonatype.org/register/namespace/), [requirements](https://central.sonatype.org/publish/requirements/), [closed-source artifacts](https://central.sonatype.org/faq/closed-source/), [publisher API](https://central.sonatype.org/publish/publish-portal-api/).
