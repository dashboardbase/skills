# Authentication

> **Load this file when:** choosing an authentication method, adding or changing the method on an endpoint that is already live, rotating a key, debugging a 401 from Dashboardbase, or configuring a datasource.

Dashboardbase polls your endpoint over HTTPS at a fixed `refreshInterval`. You decide how Dashboardbase authenticates to you.

**Every request Dashboardbase makes already carries your workspace's Endpoint Secret** in the `x-dashboardbase-secret` header — you do not configure anything for that to happen. Verifying it is the simplest way to protect an endpoint, and it is the recommended starting point. The other methods below still work, and anything you configure on the datasource is sent *in addition to* the secret.

## Methods at a glance

| Method | What Dashboardbase sends | Configure on the datasource? | Use when |
| --- | --- | --- | --- |
| **Endpoint Secret** (recommended) | `x-dashboardbase-secret` on every request, plus `x-dashboardbase-secret-previous` during a rotation | **No** — it is already being sent | The default. Nothing to agree on, and rotation has a built-in grace window |
| **API key header** | The header name and value you configure, conventionally `x-api-key` | Yes — one header per datasource | You want a credential scoped to one datasource, or a gateway already expects an API key header |
| **Basic auth** | `Authorization: Basic <base64(user:pass)>` | Yes — username and password per datasource | The system you are exposing already speaks Basic auth |
| **Custom `Authorization` header** | Whatever you configure, e.g. `Authorization: Bearer <token>` | Yes — one header per datasource | You already issue Bearer tokens or a vendor-specific scheme |

Each method below is written the same way — what arrives, when to use it, how to verify it, what to configure in Dashboardbase, and how to rotate it — so they can be compared line by line. Methods combine: an endpoint may accept more than one, which is what makes the migration in "Changing or adding an authentication method on a live endpoint" possible.

## Endpoint Secret (recommended default)

**What Dashboardbase sends.** Your workspace has one generated secret, shown at **app.dashboardbase.com/workspace**. It arrives in the `x-dashboardbase-secret` header on every poll, whether or not the datasource has any credentials configured. During a rotation the previous value arrives alongside it in `x-dashboardbase-secret-previous`.

**Use when.** Almost always — this is the recommended starting point:
- **Nothing to set up.** No key to generate, no header name to agree on, no per-datasource configuration. The header is already arriving.
- **One value for the whole workspace.** Every widget endpoint verifies the same secret.
- **Rotation without downtime.** See the grace window below.

**Verify it.** Put the secret in your environment as `DASHBOARDBASE_ENDPOINT_SECRET` and compare it on every request.

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

- Compare with a constant-time comparison where your language offers one (`crypto.timingSafeEqual`, `hmac.compare_digest`, `CryptographicOperations.FixedTimeEquals`).
- **Accept either header.** During a rotation Dashboardbase sends the new value in `x-dashboardbase-secret` and the old one in `x-dashboardbase-secret-previous`. Checking both is what lets you redeploy at your own pace.
- Return `401 Unauthorized` on mismatch; **do not redirect** (HTTP redirects on auth failure break the poll).
- Read it from the environment. Never commit it, and never log it.

**Configure on the datasource.** Nothing at all. This is the whole point of the method — there is no datasource field to fill in, and adding one changes nothing about what arrives.

**Rotate it.** With the built-in grace window below — no datasource edit, and no coordinated deploy.

### Rotating the Endpoint Secret

1. Regenerate in the workspace settings. Dashboardbase immediately starts sending the new value, and keeps sending the old one as `x-dashboardbase-secret-previous` for **24 hours**.
2. Update `DASHBOARDBASE_ENDPOINT_SECRET` in your environment and deploy, any time inside that window.
3. After 24 hours only the new value is sent.

If your endpoint checks both headers as shown above, there is no window in which a deployed endpoint returns 401.

## API key header

**What Dashboardbase sends.** A custom header you choose, carrying the value you configured on the datasource. `x-api-key` is the convention; any header name works.

```bash
curl -H "x-api-key: $DASHBOARDBASE_KEY" https://your-api.example.com/widgets/mrr
```

**Use when.**
- You want a credential scoped to a single datasource rather than one shared across the workspace.
- You are fronting the endpoint with a gateway that already expects an API key header.

**Verify it.**
- Compare the header value with `Ordinal` (case-sensitive) string comparison.
- Return `401 Unauthorized` on mismatch; do not redirect.
- Store keys in a secret manager / environment variable; never commit them.
- Reject `x-api-key` passed as a query string — keys belong in headers only.

**Configure on the datasource.** Yes — the header name and value, on **every** datasource pointing at this endpoint. A key configured on one datasource is not visible to another.

**Rotate it.** Manually, via "Rotation procedure for configured credentials" below.

## Basic auth

**What Dashboardbase sends.** `Authorization: Basic <base64(user:pass)>`, built from the username and password configured on the datasource.

```bash
curl -u "user:pass" https://your-api.example.com/widgets/mrr
```

**Use when.**
- You're integrating an existing system that already exposes Basic auth (most web frameworks support it out of the box).
- You don't need per-datasource key rotation.

**Avoid when.**
- Rotation matters (changing the password forces every downstream consumer to update).
- Users tend to reuse credentials across services.

**Verify it.**
- Let your framework's Basic auth middleware decode the header; check the username *and* the password, constant-time where your language offers it.
- Return `401 Unauthorized` on mismatch. Do not send a `WWW-Authenticate` challenge that redirects to a login page — Dashboardbase does not follow it.

**Configure on the datasource.** Yes — username and password, on every datasource pointing at this endpoint.

**Rotate it.** Manually, via "Rotation procedure for configured credentials" below.

## Custom `Authorization` header (e.g. Bearer)

**What Dashboardbase sends.** Any header you specify, including `Authorization: Bearer <token>` or a vendor-specific scheme. The value is sent verbatim on every poll — Dashboardbase never refreshes or re-issues a token.

```bash
curl -H "Authorization: Bearer $TOKEN" https://your-api.example.com/widgets/mrr
```

**Use when.**
- You already issue Bearer tokens (OAuth client credentials, JWT, etc.).
- You want short-lived tokens with refresh — though Dashboardbase does not refresh tokens; it just sends what you configured. For rotation, prefer the Endpoint Secret.

**Verify it.**
- Validate the token exactly as the rest of your API does, then return `401` on failure without redirecting.
- Remember that an expiring token 401s **every** poll from the moment it expires, and nothing renews it. If the token is short-lived, either issue a long-lived one for this datasource or use the Endpoint Secret instead.

**Configure on the datasource.** Yes — as a header, on every datasource pointing at this endpoint.

**Rotate it.** Manually, via "Rotation procedure for configured credentials" below — and before the token expires, not after.

## Changing or adding an authentication method on a live endpoint

> **Read this section when:** the endpoint is already being polled and the task is to add a second method, swap one method for another, drop one, or put auth in front of an endpoint that has been open.

An auth change is different from every other change to a live endpoint. A mis-shaped payload breaks one widget; a `401` breaks **every** widget polling that endpoint, on every dashboard, at the next refresh. And the change is split in two: the check lives in your code, while the credential (for every method except the Endpoint Secret) lives on the datasource in Dashboardbase. The two halves land at different moments, and often through different people, so the order matters more than the code does.

### Widen, switch, narrow — always in that order

1. **Widen (code).** Deploy the endpoint so it accepts **both** what it accepts today and the new method. Nothing breaks: the credential currently arriving still works.
2. **Switch (configuration).** Update every datasource that points at this endpoint — in Dashboardbase, open the dashboard editor, select the widget, and edit its datasource. **Skip this step entirely when the new method is the Endpoint Secret**: it is already being sent on every request.
3. **Confirm.** Give it one refresh interval and check the widgets still render — see "Verify the change" below.
4. **Narrow (code).** Only now, remove acceptance of the old credential and redeploy.

That is two deploys, and the gap between them is the point: for as long as it lasts, either credential works, so no poll can land on a version of the endpoint that rejects what Dashboardbase is sending. Doing step 4 before step 2 is the reliable way to red every widget on the dashboard.

### Which ask is which

| The ask | In your code | In Dashboardbase | Watch out for |
| --- | --- | --- | --- |
| **Add a second method** alongside the one already in use | Accept either credential | Configure the new credential on each datasource — nothing when it is the Endpoint Secret | Stop after step 2; there is nothing to narrow. Auth is now "any one of these passes" |
| **Replace one method with another** | Widen, then narrow — two deploys | Configure the new credential, then clear the old one | The full four steps. Clearing the old credential in the app before step 4 is harmless; doing step 4 first is not |
| **Remove a method**, leaving the others | Drop that check only | Clear that credential from each datasource | Confirm at least one method still protects the endpoint. Dropping the last one leaves it open to anyone with the URL |
| **Put auth in front of an open endpoint** | Add the check and deploy | Endpoint Secret: nothing. Any other method: configure the credential **first** | Configuring first is the widen step here — the open endpoint ignores an unexpected header, so it is safe to configure ahead of the deploy |

### One endpoint, many datasources

A single endpoint can back several widgets, on several dashboards, in more than one organisation — and configured credentials are stored **per datasource**, not per endpoint. So a swap or a removal means editing every datasource that points at it, and missing one leaves that widget 401ing after the narrow step. Before you start, list them: `.dashboardbase/*.json` in the repo shows which widgets are wired to the endpoint, and the dashboard editor shows the rest.

The Endpoint Secret is the exception, and the reason it is recommended: one value for the whole workspace, already arriving, nothing per-datasource to hunt down.

### The setup file does not change

Credentials never live in `.dashboardbase/<slug>.json` — `POST /tools/v1/setup-links` rejects a file carrying anything credential-shaped. So an auth change is **not** a re-import: it is a code deploy plus a datasource edit in the app. Only a change to the endpoint's URL touches the setup file (see `references/modifying-endpoints.md` § "Does the setup file need updating?").

### Verify the change

1. `curl` with the new credential → `200`.
2. `curl` with **no** credentials at all → `401`. This is the one that catches an endpoint that never checked what it was configured to expect.
3. `curl` with the old credential → `200` while widened, `401` after narrowing. Both are correct at the right moment; know which one you expect right now.
4. Run the datasource **Test** in the editor. Dashboardbase also probes the endpoint with every credential stripped and reports whether it answered anyway — configuring a credential says nothing about whether your code checks it.
5. Watch one full refresh interval on the dashboard before calling it done. The widget, not the `curl`, is the acceptance test.

### If the widgets go red

Re-widen first, diagnose after. Redeploy the endpoint accepting the old credential again — the dashboard recovers on the next poll — and work out what the new path rejects from a healthy dashboard. A widget in an error state is what the room, or the wall-mounted TV, is looking at.

## Rotation procedure for configured credentials

The Endpoint Secret rotates with a built-in grace window (above). For an API key, Basic auth, or a custom `Authorization` header you configured yourself:

1. Generate a new key in your secret store.
2. Update your endpoint to accept **both** the old and the new key for a grace period.
3. Update the Dashboardbase datasource to use the new key.
4. Confirm widgets refresh successfully.
5. Remove old-key acceptance from your endpoint.

That is the same widen → switch → narrow shape as changing the method itself — a rotation is just a change of method to the same method.

## Common mistakes

- **Assuming a configured credential means the endpoint is protected.** Setting Basic auth on the datasource tells Dashboardbase what to send; it does not make your code check it. Dashboardbase probes your endpoint with no credentials during validation and will tell you if it answered anyway.
- **Accepting keys via query string.** Logged in access logs and CDN caches; rejected by Dashboardbase by convention.
- **Returning `200 OK` for an unauthorised request with an empty body.** Dashboardbase will render "no data"; return `401` instead.
- **Redirecting on auth failure.** Dashboardbase does not follow redirects on the poll path.
- **Checking only `x-dashboardbase-secret` and not `x-dashboardbase-secret-previous`.** Your endpoint will 401 for up to 24 hours after a rotation.
- **Storing secrets in plaintext config files committed to git.** Always use a secret manager or environment variable.
- **Dropping the old credential in the deploy that adds the new one.** There is no moment when both are configured, so the dashboard 401s until someone updates every datasource. Widen, switch, then narrow.
- **Changing the code and forgetting the datasource, or the reverse.** An auth change is only finished when both halves are done and one refresh interval has passed cleanly.
- **Configuring the Endpoint Secret as a datasource header.** It is already sent on every request, and Dashboardbase overwrites any datasource header of that name with the workspace value — a hand-entered one is replaced, not merged, so your endpoint never sees it. Leave it out.
- **Updating one datasource and assuming the rest followed.** Credentials are per datasource — every widget pointing at the endpoint needs the edit.
