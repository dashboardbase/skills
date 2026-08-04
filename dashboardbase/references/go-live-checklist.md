# Go-live checklist

> **Load this file when:** preparing a Dashboardbase widget endpoint for production, before pointing real users at it.

Walk through this list before declaring the integration ready.

## Endpoint hosting

- [ ] Endpoint is reachable over `https://` (not `http://`) with a valid certificate.
- [ ] DNS is stable; the hostname will not change.
- [ ] No private-network or `localhost` URLs.
- [ ] Endpoint responds in under 2 seconds p95, well under the 5-second hard timeout.

## Response correctness

- [ ] Response JSON validates against the widget's schema — via the `validate_widget_response` tool if it is available (pass the full response body), otherwise against `assets/schemas/<widget>.json` (see `references/<widget>.md`).
- [ ] No extra fields outside the schema (`additionalProperties: false` will reject them).
- [ ] All enum values use exact case (`Success`, not `success`).
- [ ] Action URLs are absolute `https://`.
- [ ] Sort order is deterministic across polls.
- [ ] Non-finite numbers (`NaN`, `Infinity`) are converted before serialisation.

## Authentication

- [ ] Auth is configured (recommended: API key via `x-api-key` header — see `references/authentication.md`).
- [ ] Secret is stored in a secret manager or environment variable; not in committed config.
- [ ] Endpoint returns `401` (not `302` redirect) on bad credentials.
- [ ] Key rotation procedure is documented.

## Refresh interval

- [ ] Configured `refreshInterval` (`1m` / `5m` / `10m` / `30m`) is realistic given upstream cost.
- [ ] Account for fan-out: how many dashboards point at this endpoint × poll rate ≤ upstream rate limit.
- [ ] Server-side cache in place if the underlying query is expensive.

## Reliability

- [ ] Endpoint has monitoring: latency, error rate, request volume.
- [ ] Alerting is wired for sustained `5xx` or response time over 2 seconds.
- [ ] Endpoint behaves predictably under load: rate limiting allows Dashboardbase's poll rate.
- [ ] Database queries supporting this endpoint use indexed columns and are not expected to slow down with scale.

## Setup file

- [ ] If publishing a dashboard via setup file: the file validates — via the `validate_setup_file` tool if it is available (no org ID and no credentials needed), otherwise via `POST /bff/v1/organizations/{orgId}/setup-files/validate`.
- [ ] `refreshInterval` in the setup file matches upstream rate limits.
- [ ] Datasource baseUrls in the setup file are correct.
- [ ] No credentials in the setup file.

## Events (if used)

- [ ] Webhook secret is stored in a secret manager.
- [ ] Sender is server-side only (never client-side).
- [ ] Deduplication is in place if the trigger source can retry.

## Documentation handover

- [ ] An operator can read your integration doc and answer: where is the endpoint, where is the secret, how do I rotate it, how do I know it's healthy, who's paged when it isn't.
