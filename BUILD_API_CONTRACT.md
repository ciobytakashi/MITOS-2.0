# MITOS Build API — Initial Contract

Base URL is configured by the user/admin. All endpoints except `/health` require a short-lived device bearer token over HTTPS.

## `GET /health`

Returns service health and API compatibility. Example:

```json
{"service":"mitos-build-engine","status":"ok","api_version":"v1"}
```

## `POST /v1/builds`

Multipart upload fields:
- `project`: source ZIP archive
- `app_name`: display name
- `package_id`: Android application ID
- `provider`: must be `mitos` for the independent engine

Returns `202 Accepted` with `{ "build_id": "...", "status": "queued" }`.

The endpoint must reject malformed archives, oversized uploads, unsupported project types, and invalid package IDs before queuing.

## `GET /v1/builds/{build_id}`

Returns status, timestamps, detected project type, safe diagnostic summary, and artifact metadata when complete. It must not expose host paths, environment variables, signing secrets, or raw internal exceptions.

## `GET /v1/builds/{build_id}/logs`

Returns paginated or cursor-based build log lines. Secrets and sensitive environment values must be redacted before persistence or delivery.

## `GET /v1/builds/{build_id}/artifact`

Streams the completed APK/AAB only when the authenticated device owns the job and the artifact has not expired.

## `POST /v1/builds/{build_id}/cancel`

Requests cancellation. A worker must terminate the isolated build process and clean its workspace.

## API compatibility

Use a versioned `/v1` prefix. Breaking changes require a new version. Client timeouts and retry behavior must use idempotency keys for upload submission to avoid duplicate jobs.
