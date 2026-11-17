# MITOS 2.0 status

## Present in this starter
- [x] Minimalist Android WebView shell.
- [x] APK Builder intake screen (metadata registration only; no compilation).
- [x] Web App Studio for standalone HTML files.
- [x] Local project metadata and JSON backup/restore.
- [x] Initial MITOS-owned build-engine architecture, API contract, security requirements, and roadmap.
- [x] Manual GitHub Actions workflow for producing a debug test APK.

## Not implemented or not verified
- [ ] APK Builder compilation of uploaded source projects.
- [ ] ZIP extraction and multi-file web app runtime.
- [ ] IndexedDB-backed file storage and quota management.
- [ ] Per-app storage isolation beyond current app records.
- [ ] MITOS API endpoint configuration and authenticated client connection.
- [ ] Independent MITOS API, queue, job database, and isolated build workers.
- [ ] Archive validation and project-type adapters.
- [ ] Production signing service, APK/AAB verification, and artifact retention.
- [ ] Optional explicit GitHub provider adapter inside the app.
- [ ] Automated tests and Android 16 device testing.
- [ ] Successful build and clean-install verification in the user's environment.

The GitHub Actions workflow is a manual/commit-triggered development build path only. It does not make GitHub the MITOS build engine and does not upload projects from the MITOS app.
