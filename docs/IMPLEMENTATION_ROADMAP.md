# MITOS 2.0 — Build Engine Architecture

## Product decision

The MITOS-owned build engine is the default and primary build provider. GitHub Actions is an optional provider that a user must deliberately select for an individual build. MITOS must never silently send source code to GitHub or automatically fall back to it.

## Components

1. **MITOS Android client** — mobile-first project intake, build configuration, job progress, logs, and artifact download. The client does not contain the Android SDK or attempt heavyweight builds on the phone.
2. **MITOS API** — authenticates devices, validates uploads, creates build jobs, reports status, and serves completed artifacts.
3. **Queue** — stores job IDs and scheduling metadata; it does not store signing secrets in job payloads.
4. **Isolated build workers** — ephemeral containers/VMs with a pinned JDK, Gradle, Android SDK platform and build-tools versions. Each job gets a fresh workspace, resource limits, no privileged container access, and restricted network access.
5. **Artifact/signing service** — signs only after a successful build, keeps signing keys outside source workspaces, and records artifact hashes and build metadata.
6. **Optional GitHub adapter** — invoked only after an explicit per-build provider choice and clear confirmation of what source is sent.

## Build lifecycle

`uploaded -> validated -> queued -> building -> signing -> completed`

Terminal alternatives: `rejected`, `failed`, `cancelled`, `expired`.

Every state transition is recorded with a timestamp. Build logs are streamed or paginated separately from job metadata. Artifacts have an expiry policy and a SHA-256 digest.

## Security requirements

- Require TLS for all non-local API traffic.
- Authenticate devices with revocable tokens; do not put permanent secrets in the APK.
- Enforce upload size, archive entry count, expanded size, path traversal, symlink, and compression-ratio limits.
- Reject archives containing path traversal or unsupported project layouts.
- Build untrusted source in disposable, unprivileged sandboxes with CPU, memory, disk, and time limits.
- Do not mount the Docker socket or host filesystem into build workers.
- Deny worker access to cloud metadata endpoints and internal control-plane networks.
- Keep signing keys in a separate secret store; never copy them into build workspaces or logs.
- Scan and validate generated APK/AAB files; record package ID, version, signing certificate fingerprint, and SHA-256.
- Never silently change provider or upload source to a third party.

## Project support policy

A file extension alone does not make a project buildable. The importer must identify project type and required toolchain, inspect its manifest/build files, and report unsupported or missing dependencies before queuing. Initial server support should prioritize standard Android Gradle projects. Web-to-APK packaging and other ecosystems should be separate, explicitly versioned build adapters.

## Local builds

On-device builds are a later optimization, not a prerequisite for the first release. Android phones are not assumed to host a complete Android SDK/Gradle toolchain. The app may perform lightweight validation and project inspection locally.
