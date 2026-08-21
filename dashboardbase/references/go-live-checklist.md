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

- [ ] Auth is enforced (recommended: verify the `x-dashboardbase-secret` header — see `references/authentication.md`).
- [ ] Both `x-dashboardbase-secret` and `x-dashboardbase-secret-previous` are accepted, so a rotation does not 401 the endpoint.
- [ ] Secret is stored in a secret manager or environment variable (`DASHBOARDBASE_ENDPOINT_SECRET`); not in committed config.
- [ ] Endpoint returns `401` (not `302` redirect) on bad credentials.
- [ ] An unauthenticated request to the endpoint returns `401`, not data — confirm with the datasource "Test" in the editor.
- [ ] Key rotation procedure is documented.
- [ ] If the auth method changed or a method was added: every datasource pointing at this endpoint was updated and re-tested, not just the first one.
- [ ] Acceptance of the previous credential was removed only after those datasources were confirmed working.

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

## Alerts (if used)

- [ ] The endpoint raises `alert` on the threshold **and clears it** when the metric is healthy.
- [ ] `alert.level` is lowercase, and `active` / `level` / `message` are all present.
- [ ] The message carries the observed value and the threshold, not just "high".
- [ ] Alerts are enabled on the widget in the dashboard editor — the endpoint alone does nothing.
- [ ] The dashboard is published; drafts are not polled for alerts.
- [ ] The people who should hear about it have alert notifications switched on.

## Events (if used)

- [ ] Webhook secret is stored in a secret manager.
- [ ] Sender is server-side only (never client-side).
- [ ] Deduplication is in place if the trigger source can retry.

## Documentation handover

- [ ] An operator can read your integration doc and answer: where is the endpoint, where is the secret, how do I rotate it, how do I know it's healthy, who's paged when it isn't.
