# Authentication

> **Load this file when:** choosing an authentication method, rotating a key, debugging a 401 from Dashboardbase, or configuring a datasource.

Dashboardbase polls your endpoint over HTTPS at a fixed `refreshInterval`. You decide how Dashboardbase authenticates to you.

**Every request Dashboardbase makes already carries your workspace's Endpoint Secret** in the `x-dashboardbase-secret` header — you do not configure anything for that to happen. Verifying it is the simplest way to protect an endpoint, and it is the recommended starting point. The other methods below still work, and anything you configure on the datasource is sent *in addition to* the secret.

## Recommended: verify the Endpoint Secret

Your workspace has one generated secret, shown at **app.dashboardbase.com/workspace**. Put it in your environment as `DASHBOARDBASE_ENDPOINT_SECRET` and compare it on every request.

```bash
curl -H "x-dashboardbase-secret: $DASHBOARDBASE_ENDPOINT_SECRET" https://your-api.example.com/widgets/mrr
```

```js
const EXPECTED = process.env.DASHBOARDBASE_ENDPOINT_SECRET;

function verifyDashboardbase(req, res, next) {
  const sent = req.get("x-dashboardbase-secret");
  const previous = req.get("x-dashboardbase-secret-previous");
  if (sent !== EXPECTED && previous !== EXPECTED) return res.sendStatus(401);
  next();
}
```

Why this is the default:
- **Nothing to set up.** No key to generate, no header name to agree on, no per-datasource configuration. The header is already arriving.
- **One value for the whole workspace.** Every widget endpoint verifies the same secret.
- **Rotation without downtime.** See the grace window below.

Implementation notes:
- Compare with a constant-time comparison where your language offers one (`crypto.timingSafeEqual`, `hmac.compare_digest`, `CryptographicOperations.FixedTimeEquals`).
- **Accept either header.** During a rotation Dashboardbase sends the new value in `x-dashboardbase-secret` and the old one in `x-dashboardbase-secret-previous`. Checking both is what lets you redeploy at your own pace.
- Return `401 Unauthorized` on mismatch; **do not redirect** (HTTP redirects on auth failure break the poll).
- Read it from the environment. Never commit it, and never log it.

### Rotating the Endpoint Secret

1. Regenerate in the workspace settings. Dashboardbase immediately starts sending the new value, and keeps sending the old one as `x-dashboardbase-secret-previous` for **24 hours**.
2. Update `DASHBOARDBASE_ENDPOINT_SECRET` in your environment and deploy, any time inside that window.
3. After 24 hours only the new value is sent.

If your endpoint checks both headers as shown above, there is no window in which a deployed endpoint returns 401.

## Alternative 1: API key via `x-api-key` header

Dashboardbase sends a custom header you choose; your endpoint validates it.

```bash
curl -H "x-api-key: $DASHBOARDBASE_KEY" https://your-api.example.com/widgets/mrr
```

Use when:
- You want a credential scoped to a single datasource rather than one shared across the workspace.
- You are fronting the endpoint with a gateway that already expects an API key header.

Implementation notes:
- Compare the header value with `Ordinal` (case-sensitive) string comparison.
- Return `401 Unauthorized` on mismatch; do not redirect.
- Store keys in a secret manager / environment variable; never commit them.
- Reject `x-api-key` passed as a query string — keys belong in headers only.

## Alternative 2: Basic auth

Dashboardbase sends `Authorization: Basic <base64(user:pass)>`.

```bash
curl -u "user:pass" https://your-api.example.com/widgets/mrr
```

Use when:
- You're integrating an existing system that already exposes Basic auth (most web frameworks support it out of the box).
- You don't need per-datasource key rotation.

Avoid when:
- Rotation matters (changing the password forces every downstream consumer to update).
- Users tend to reuse credentials across services.

## Alternative 3: Custom Authorization header (e.g. Bearer)

Dashboardbase can send any header you specify, including `Authorization: Bearer <token>` or a vendor-specific scheme.

```bash
curl -H "Authorization: Bearer $TOKEN" https://your-api.example.com/widgets/mrr
```

Use when:
- You already issue Bearer tokens (OAuth client credentials, JWT, etc.).
- You want short-lived tokens with refresh — though Dashboardbase does not refresh tokens; it just sends what you configured. For rotation, prefer the Endpoint Secret.

## Rotation procedure for configured credentials

The Endpoint Secret rotates with a built-in grace window (above). For an API key or Basic auth you configured yourself:

1. Generate a new key in your secret store.
2. Update your endpoint to accept **both** the old and the new key for a grace period.
3. Update the Dashboardbase datasource to use the new key.
4. Confirm widgets refresh successfully.
5. Remove old-key acceptance from your endpoint.

## Common mistakes

- **Assuming a configured credential means the endpoint is protected.** Setting Basic auth on the datasource tells Dashboardbase what to send; it does not make your code check it. Dashboardbase probes your endpoint with no credentials during validation and will tell you if it answered anyway.
- **Accepting keys via query string.** Logged in access logs and CDN caches; rejected by Dashboardbase by convention.
- **Returning `200 OK` for an unauthorised request with an empty body.** Dashboardbase will render "no data"; return `401` instead.
- **Redirecting on auth failure.** Dashboardbase does not follow redirects on the poll path.
- **Checking only `x-dashboardbase-secret` and not `x-dashboardbase-secret-previous`.** Your endpoint will 401 for up to 24 hours after a rotation.
- **Storing secrets in plaintext config files committed to git.** Always use a secret manager or environment variable.
