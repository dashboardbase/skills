# Authentication

> **Load this file when:** choosing an authentication method, rotating a key, debugging a 401 from Dashboardbase, or configuring a datasource.

Dashboardbase polls your endpoint over HTTPS at a fixed `refreshInterval`. You decide how Dashboardbase authenticates to you. Three methods are supported.

## Recommended: API key via `x-api-key` header

Dashboardbase sends a custom header you choose; your endpoint validates it. This is the **recommended default** for new integrations.

```bash
curl -H "x-api-key: $DASHBOARDBASE_KEY" https://your-api.example.com/widgets/mrr
```

Why this is the default:
- **Easy to rotate.** Generate a new key, deploy, swap in Dashboardbase, revoke the old.
- **No escaping issues.** Unlike Basic auth, no `:` separator or base64 step.
- **Per-datasource scope.** Each Dashboardbase datasource gets its own key — compromise of one does not affect others.
- **Not logged in URLs.** Headers don't appear in access logs the way query strings do.

Implementation notes:
- Compare the header value with `Ordinal` (case-sensitive) string comparison.
- Return `401 Unauthorized` on mismatch; **do not redirect** (HTTP redirects on auth failure break the poll).
- Store keys in a secret manager / environment variable; never commit them.
- Reject `x-api-key` passed as a query string — keys belong in headers only.

## Alternative 1: Basic auth

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

## Alternative 2: Custom Authorization header (e.g. Bearer)

Dashboardbase can send any header you specify, including `Authorization: Bearer <token>` or a vendor-specific scheme.

```bash
curl -H "Authorization: Bearer $TOKEN" https://your-api.example.com/widgets/mrr
```

Use when:
- You already issue Bearer tokens (OAuth client credentials, JWT, etc.).
- You want short-lived tokens with refresh — though Dashboardbase does not refresh tokens; it just sends what you configured. For rotation, prefer `x-api-key`.

## Rotation procedure (recommended pattern)

1. Generate a new key in your secret store.
2. Update your endpoint to accept **both** the old and the new key for a grace period.
3. Update the Dashboardbase datasource to use the new key.
4. Confirm widgets refresh successfully.
5. Remove old-key acceptance from your endpoint.

## Common mistakes

- **Accepting keys via query string.** Logged in access logs and CDN caches; rejected by Dashboardbase by convention.
- **Returning `200 OK` for an unauthorised request with an empty body.** Dashboardbase will render "no data"; return `401` instead.
- **Redirecting on auth failure.** Dashboardbase does not follow redirects on the poll path.
- **Mixing schemes.** Pick one per datasource. Do not require both Basic and `x-api-key` simultaneously.
- **Storing keys in plaintext config files committed to git.** Always use a secret manager or environment variable.
