# Authentication

> **Load this file when:** choosing an authentication method, adding or changing the method on an endpoint that is already live, rotating a key, debugging a 401 from Dashboardbase, or configuring a datasource.

Dashboardbase polls your endpoint over HTTPS at a fixed `refreshInterval`. You decide how Dashboardbase authenticates to you.

**Every request Dashboardbase makes is already signed with your workspace's Ed25519 key**, and already carries your workspace's Endpoint Secret. You configure nothing for either to happen. Verifying one of them is how you protect an endpoint.

**Start with request signing.** You verify against a public key committed to your repository — there is no shared secret to store, leak, or rotate on your side, and a signature is bound to the method, path and timestamp rather than being one static value that works anywhere. The Endpoint Secret is the alternative if you would rather set a single environment variable and be done; it is genuinely simpler to write, and it remains fully supported.

Both arrive on every request. Verify whichever one you implemented and ignore the other — **and implement exactly one**. The other methods below still work, and anything you configure on the datasource is sent *in addition to* both.

## Methods at a glance

| Method | What Dashboardbase sends | Configure on the datasource? | Use when |
| --- | --- | --- | --- |
| **Request signing (Ed25519)** (recommended) | `x-dashboardbase-workspace`, `x-dashboardbase-timestamp` and `x-dashboardbase-signature` on every request, plus `x-dashboardbase-signature-previous` during a rotation | **No** — it is already being sent | **The default.** No secret to hold: you verify against a public key committed to your repository |
| **Endpoint Secret** | `x-dashboardbase-secret` on every request, plus `x-dashboardbase-secret-previous` during a rotation | **No** — it is already being sent | You would rather set one environment variable than commit a key file. Fewer lines to write, and rotation has a built-in grace window |
| **API key header** | The header name and value you configure, conventionally `x-api-key` | Yes — one header per datasource | You want a credential scoped to one datasource, or a gateway already expects an API key header |
| **Basic auth** | `Authorization: Basic <base64(user:pass)>` | Yes — username and password per datasource | The system you are exposing already speaks Basic auth |
| **Custom `Authorization` header** | Whatever you configure, e.g. `Authorization: Bearer <token>` | Yes — one header per datasource | You already issue Bearer tokens or a vendor-specific scheme |

Each method below is written the same way — what arrives, when to use it, how to verify it, what to configure in Dashboardbase, and how to rotate it — so they can be compared line by line. Methods combine: an endpoint may accept more than one, which is what makes the migration in "Changing or adding an authentication method on a live endpoint" possible.

## Request signing (Ed25519) — recommended default

> **Pick one.** Verify the signature **or** the Endpoint Secret — not both. Two verification routines is
> twice the surface to get subtly wrong, and a signature check that fails does so silently and closed:
> you get a `401` with no explanation, and it will look like our bug rather than yours.

> **You need your workspace's public key.** A plan or issue generated from Dashboardbase already
> contains it — look for "Verification key" in the Security section and use that. Working without one?
> Ask for the key itself: it is shown at **app.dashboardbase.com/workspace**, ready to copy, and can
> also be printed with `curl https://api.dashboardbase.com/keys/<workspace-id>.pem`. Either way you want
> the three-line `-----BEGIN PUBLIC KEY-----` block, not an id.
>
> Get it before writing the verification code. Scaffolding around a placeholder produces something that
> cannot verify, and the failure is a silent `401`.

**What Dashboardbase sends.** Every request carries two extra headers, whether or not the datasource
has any credentials configured:

```
x-dashboardbase-workspace: org_A1b2C3d4E5f6
x-dashboardbase-timestamp: 1754812800
x-dashboardbase-signature: <base64 of a 64-byte Ed25519 signature>
```

The signature covers a canonical message built from the request:

```
{METHOD}|{SCHEME}://{HOST}{PATH}|{UNIX_TIMESTAMP}
e.g.  GET|https://your-api.example.com/widgets/mrr|1754812800
```

- **Method** uppercased.
- **Scheme, host and path** — `https://your-api.example.com/widgets/mrr`. The host is in the message so
  a signature is only valid against the endpoint it was issued for. **Build it from your own configured
  origin, never from the inbound `Host` header** — the caller controls that header, so trusting it would
  undo the binding.
- **No query string.** Anything appended, such as `?dateRange=Last30Days`, is *not* covered.
  **Never scope authorization on a query parameter** — the signature proves the request came from us, it
  does not authorise a particular query.
- **No trailing slash** — `/widgets/mrr/` is signed as `/widgets/mrr`.
- **Unix seconds**, not milliseconds.

**Use when.**
- **You do not want to hold a secret.** The public key is safe to commit; there is no environment
  variable to set, no value to leak, and nothing to rotate on your side unless we rotate first.
- **You want proof of origin bound to the request.** A signature covers the method, the full endpoint
  URL and a timestamp, so it is useless against a different endpoint and expires within minutes. An
  Endpoint Secret header is the same value on every request to every endpoint, forever.
- **Your workspace has more than one team deploying endpoints.** Nobody has to be handed a secret.

**Avoid when.** You have no Dashboardbase account yet, or you would rather set one environment variable
and be done — the Endpoint Secret is a few lines shorter to write and needs no key file.

**Verify it.** A plan or issue generated from Dashboardbase already contains your workspace's public
key — paste it straight into the constant below. Working without one? Print it and copy the output:

```bash
curl https://api.dashboardbase.com/keys/<workspace-id>.pem
```

There is no key file to manage: it is three lines, so it lives in the code that uses it.

```js
import { createPublicKey, verify } from "node:crypto";

// This is a public key. It is safe to commit.
const KEY = createPublicKey(`-----BEGIN PUBLIC KEY-----
MCowBQYDK2VwAyEA<your workspace's key>
-----END PUBLIC KEY-----`);
const TOLERANCE_SECONDS = 300;
// Your own public origin. Hardcode or configure it — never derive it from the request's Host header.
const ORIGIN = "https://your-api.example.com";

function verifyDashboardbase(req, res, next) {
  const ts = Number(req.get("x-dashboardbase-timestamp"));
  const sig = req.get("x-dashboardbase-signature");
  // Sent for 24 hours after a rotation, signed with the key you may not have replaced yet.
  const prev = req.get("x-dashboardbase-signature-previous");
  if (!ts || !sig) return res.sendStatus(401);
  if (Math.abs(Date.now() / 1000 - ts) > TOLERANCE_SECONDS) return res.sendStatus(401);

  const check = (path, signature) =>
    signature && verify(null, Buffer.from(`${req.method.toUpperCase()}|${ORIGIN}${path}|${ts}`), KEY,
                        Buffer.from(signature, "base64"));

  const raw = (req.originalUrl.split("?")[0] || "/").replace(/(.)\/$/, "$1");

  // Frameworks differ on whether the path they hand you is percent-decoded, so try both. Skip the
  // decoded form when it contains an encoded separator: decoding %2F would merge segments and let a
  // signature issued for one route validate a request to another.
  const decoded = /%2f|%5c/i.test(raw) ? raw : decodeURIComponent(raw);
  const ok = [raw, decoded].some(p => check(p, sig) || check(p, prev));

  if (!ok) return res.sendStatus(401);
  next();
}
```

- **Use the raw, undecoded path.** Express's `req.path` is raw, but ASP.NET Core's `Request.Path`,
  Django's `request.path`, Flask's `request.path` and Go's `r.URL.Path` are percent-**decoded**. The
  raw accessors are `req.originalUrl.split("?")[0]`,
  `HttpContext.Features.Get<IHttpRequestFeature>().RawTarget` split on `?`, `request.scope["raw_path"]`
  in ASGI, and `r.URL.EscapedPath()` in Go. Checking both forms as above means it does not matter which
  one you reached for — except for an encoded separator (`%2F`), where the fallback is skipped on
  purpose so decoding cannot merge path segments.
- **Allow at least 120 seconds of clock skew; 300 is the recommended value.** A request that is retried
  keeps its original timestamp, so a slow endpoint can legitimately receive one that is nearly a minute
  old. A tight window produces intermittent failures that look random.
- **Serving more than one workspace from the same endpoint? Select exactly one key.** Every request
  carries `x-dashboardbase-workspace: org_…`. Look the expected public key up by that value and verify
  against it alone. **Never loop over every key you trust** — a loop accepts a signature from any
  workspace you have ever onboarded, which throws away the per-workspace isolation and leaves you with
  the same exposure as a single shared secret. An unknown workspace id is a `401`, not a fallback.
- The signature is standard base64 (not base64url) of exactly 64 bytes.
- Return `401 Unauthorized` on mismatch; **do not redirect**.
- In C#, verification needs a library — .NET has no Ed25519. `BouncyCastle.Cryptography`'s
  `Ed25519Signer` is the usual choice. Node, Python (`cryptography`) and Go (`crypto/ed25519`) all
  verify from their standard libraries.

**Configure on the datasource.** Nothing at all — the headers are already arriving, exactly like the
Endpoint Secret.

**Test it.** Because verification fails closed and silently, use **Validate signature verification** in the
datasource editor. It sends one correctly signed request and one with a deliberately broken signature,
and tells you whether your endpoint accepted the right one and rejected the wrong one.

### Rotating the signing key

1. Rotate in the workspace settings. Dashboardbase immediately starts signing with the new key and
   keeps sending a signature from the old one in `x-dashboardbase-signature-previous` for **24 hours**.
2. Re-fetch `/keys/<workspace-id>.pem` and replace the constant, any time inside that window. You only
   ever need the one key: while the window is open we send a signature from the new key *and* one from
   the old, and the verifier above checks both — so whichever key you are holding, requests verify.
3. After 24 hours only the new key is used.

**Revoking** is the break-glass version: the old key stops being sent immediately, with no grace window.
Every endpoint still verifying the old key starts rejecting at once, so only revoke if you believe the
old key is compromised — and re-commit the new public key straight away, because dashboards will not
recover on their own.

## Endpoint Secret

**What Dashboardbase sends.** Your workspace has one generated secret, shown at **app.dashboardbase.com/workspace**. It arrives in the `x-dashboardbase-secret` header on every poll, whether or not the datasource has any credentials configured. During a rotation the previous value arrives alongside it in `x-dashboardbase-secret-previous`.

**Use when.** You want the shortest possible implementation, and holding one secret value is not a
problem for you:
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
- You want short-lived tokens with refresh — though Dashboardbase does not refresh tokens; it just sends what you configured. For rotation, prefer request signing or the Endpoint Secret.

**Verify it.**
- Validate the token exactly as the rest of your API does, then return `401` on failure without redirecting.
- Remember that an expiring token 401s **every** poll from the moment it expires, and nothing renews it. If the token is short-lived, either issue a long-lived one for this datasource or use request signing instead.

**Configure on the datasource.** Yes — as a header, on every datasource pointing at this endpoint.

**Rotate it.** Manually, via "Rotation procedure for configured credentials" below — and before the token expires, not after.

## Changing or adding an authentication method on a live endpoint

> **Read this section when:** the endpoint is already being polled and the task is to add a second method, swap one method for another, drop one, or put auth in front of an endpoint that has been open.

An auth change is different from every other change to a live endpoint. A mis-shaped payload breaks one widget; a `401` breaks **every** widget polling that endpoint, on every dashboard, at the next refresh. And the change is split in two: the check lives in your code, while the credential (for every method except the Endpoint Secret) lives on the datasource in Dashboardbase. The two halves land at different moments, and often through different people, so the order matters more than the code does.

### Widen, switch, narrow — always in that order

1. **Widen (code).** Deploy the endpoint so it accepts **both** what it accepts today and the new method. Nothing breaks: the credential currently arriving still works.
2. **Switch (configuration).** Update every datasource that points at this endpoint — in Dashboardbase, open the dashboard editor, select the widget, and edit its datasource. **Skip this step entirely when the new method is the Endpoint Secret or request signing**: both are already being sent on every request.
3. **Confirm.** Give it one refresh interval and check the widgets still render — see "Verify the change" below.
4. **Narrow (code).** Only now, remove acceptance of the old credential and redeploy.

That is two deploys, and the gap between them is the point: for as long as it lasts, either credential works, so no poll can land on a version of the endpoint that rejects what Dashboardbase is sending. Doing step 4 before step 2 is the reliable way to red every widget on the dashboard.

### Which ask is which

| The ask | In your code | In Dashboardbase | Watch out for |
| --- | --- | --- | --- |
| **Add a second method** alongside the one already in use | Accept either credential | Configure the new credential on each datasource — nothing when it is the Endpoint Secret or request signing | Stop after step 2; there is nothing to narrow. Auth is now "any one of these passes" |
| **Replace one method with another** | Widen, then narrow — two deploys | Configure the new credential, then clear the old one | The full four steps. Clearing the old credential in the app before step 4 is harmless; doing step 4 first is not |
| **Remove a method**, leaving the others | Drop that check only | Clear that credential from each datasource | Confirm at least one method still protects the endpoint. Dropping the last one leaves it open to anyone with the URL |
| **Put auth in front of an open endpoint** | Add the check and deploy | Endpoint Secret or request signing: nothing. Any other method: configure the credential **first** | Configuring first is the widen step here — the open endpoint ignores an unexpected header, so it is safe to configure ahead of the deploy |

### One endpoint, many datasources

A single endpoint can back several widgets, on several dashboards, in more than one organisation — and configured credentials are stored **per datasource**, not per endpoint. So a swap or a removal means editing every datasource that points at it, and missing one leaves that widget 401ing after the narrow step. Before you start, list them: `.dashboardbase/*.json` in the repo shows which widgets are wired to the endpoint, and the dashboard editor shows the rest.

Request signing and the Endpoint Secret are the exceptions, and the reason one of them is recommended: both are per workspace, already arriving, with nothing per-datasource to hunt down.

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
