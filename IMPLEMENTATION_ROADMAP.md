# MITOS 2.0 — Implementation Roadmap

## Phase A — Mobile client and contract
- [x] Decide independent MITOS engine is the default provider.
- [x] Keep GitHub as explicit, optional, per-build provider only.
- [x] Define initial API lifecycle and security boundaries.
- [ ] Add engine endpoint configuration and authenticated connection test to Android client.
- [ ] Implement upload, job status, logs, cancellation, and artifact download in client.

## Phase B — Independent build service
- [ ] Implement authenticated API and device enrollment/revocation.
- [ ] Implement queue, job database, and retention policy.
- [ ] Implement disposable Android Gradle build workers with pinned toolchains.
- [ ] Add archive validation and project-type detection.
- [ ] Add isolated signing service and artifact verification.
- [ ] Add deployment, monitoring, backups, and operational documentation.

## Phase C — Supported project adapters
- [ ] Standard Android Gradle projects (first supported adapter).
- [ ] HTML/CSS/JS packaging adapter.
- [ ] Kotlin/Java project compatibility checks.
- [ ] React/other frameworks only when their build requirements are available and tested.
- [ ] Flutter adapter only after its SDK and worker requirements are implemented.

## Phase D — Optional providers and local optimization
- [ ] Add GitHub Actions adapter behind an explicit per-build selection and confirmation.
- [ ] Add lightweight on-device project inspection.
- [ ] Evaluate local compilation only for supported devices and toolchains.

## Release gate
No build is labelled successful until the produced APK/AAB is verified, its digest and signing certificate are recorded, and installation is tested on a real Android device. This source package is not itself a compiled or installable APK.
