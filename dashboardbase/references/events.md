# Events

> **Load this file when:** you need to push a real-time event, notification, sound, or alert to a Dashboardbase dashboard from your own backend.

Events are real-time signals your backend pushes **to** Dashboardbase (the inverse of widget polling). An event can play a sound, show a notification, and render an emoji on dashboards subscribed to the webhook.

## Endpoint

```http
POST /webhook/events/{webhookId}
Content-Type: application/json

{
  "secret": "<shared secret configured in Dashboardbase>",
  "message": "Deploy succeeded for api-prod",
  "emoji": "🎉",
  "sound": true,
  "notification": true
}
```

`{webhookId}` is the UUID of your organization's events webhook. One webhook is created for the workspace automatically — you do not create it per dashboard or per integration. The user finds its ID and secret at **app.dashboardbase.com/workspace**, the same page as the Endpoint Secret.

### Fields

- `secret` (required) — the shared secret from the workspace page. A mismatch is rejected with `401`; no event is recorded.
- `message` (required) — short human-readable text.
- `emoji` (optional) — either a literal emoji character (e.g. `"🎉"`) **or** a short name from the table below (e.g. `"tada"`). Names are case-insensitive. Anything unrecognised is passed through as-is, so plain Unicode always works.
- `sound` (optional, default `false`) — play a sound for connected dashboard viewers.
- `notification` (optional, default `false`) — display a notification card.

### Available emoji names

You can pass the literal Unicode character, or use one of these short names. The list is the source of truth — if a new name is added on the server, it appears here automatically.

### General

| Name | Renders as |
| --- | --- |
| `heart` | ❤️ |
| `money` | 💸 |
| `rocket` | 🚀 |
| `fire` | 🔥 |
| `happy` | 😊 |

### Business

| Name | Renders as |
| --- | --- |
| `chart` | 📈 |
| `trophy` | 🏆 |
| `handshake` | 🤝 |
| `clap` | 👏 |
| `thumbsup` | 👍 |
| `crown` | 👑 |
| `star` | ⭐ |

### Ecommerce

| Name | Renders as |
| --- | --- |
| `cart` | 🛒 |
| `gift` | 🎁 |
| `package` | 📦 |
| `diamond` | 💎 |
| `sale` | 🏷️ |

### Development

| Name | Renders as |
| --- | --- |
| `bug` | 🐛 |
| `code` | 💻 |
| `merge` | 🔀 |
| `warning` | ⚠️ |
| `check` | ✅ |
| `bolt` | ⚡ |
| `gear` | ⚙️ |
| `robot` | 🤖 |
| `tada` | 🎉 |

### Colors

| Name | Renders as |
| --- | --- |
| `red` | 🔴 |
| `green` | 🟢 |
| `yellow` | 🟡 |
| `orange` | 🟠 |
| `blue` | 🔵 |
| `purple` | 🟣 |
| `brown` | 🟤 |
| `black` | ⚫ |
| `white` | ⚪ |

### Response

| Status | Meaning |
| --- | --- |
| `200` | Event accepted and queued. |
| `400` | `secret` or `message` is missing or empty. |
| `401` | The secret does not match this webhook. |
| `404` | No webhook with that ID — check `{webhookId}`. |
| `429` | Rate limited. Events are for human-scale signals, not a log stream. |

## When to send events

These are good triggers for an event:

| Trigger | Example payload |
|---|---|
| Deploy succeeded | `{ "message": "Deploy v1.42 succeeded", "emoji": "🚀", "sound": true }` |
| Deploy failed | `{ "message": "Deploy v1.42 failed", "emoji": "💥", "sound": true, "notification": true }` |
| New paying customer | `{ "message": "New $99/mo signup: Acme Corp", "emoji": "💰" }` |
| Pager alert opened | `{ "message": "ERR_RATE > 5%", "emoji": "🔥", "notification": true, "sound": true }` |
| Pager alert resolved | `{ "message": "ERR_RATE back to normal", "emoji": "✅" }` |

## Implementation pattern (Node.js)

```js
async function sendEvent(payload) {
  await fetch(`https://api.dashboardbase.com/webhook/events/${process.env.WEBHOOK_ID}`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      secret: process.env.WEBHOOK_SECRET,
      ...payload
    })
  });
}

// In your deploy pipeline:
await sendEvent({ message: "Deploy succeeded", emoji: "🚀", sound: true });
```

## Wiring events into an existing codebase

Most of the time the project already has the moment worth broadcasting — a webhook handler, a job, a deploy step. The work is adding one call to it, not building an events feature.

### 1. Get the credentials into the environment

Ask the user for the webhook ID and secret from **app.dashboardbase.com/workspace** and put them in the environment the same way the project holds its other secrets:

```bash
DASHBOARDBASE_WEBHOOK_ID=<uuid from the workspace page>
DASHBOARDBASE_WEBHOOK_SECRET=<secret from the workspace page>
```

Never put either one in a setup file, in client-side code, or in the repo. The secret authenticates the *sender* — anyone holding it can post events to the user's dashboards.

### 2. Write one sender, in the project's own idiom

Add a single function and route every event through it. Build it out of what the project already uses — its HTTP client, its config object, its logger — rather than introducing a new dependency for five lines of `POST`:

```js
export async function sendDashboardbaseEvent({ message, emoji, sound, notification }) {
  try {
    await fetch(`https://api.dashboardbase.com/webhook/events/${config.dashboardbase.webhookId}`, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ secret: config.dashboardbase.webhookSecret, message, emoji, sound, notification }),
      signal: AbortSignal.timeout(3000)
    });
  } catch (err) {
    logger.warn({ err }, "Dashboardbase event not sent");
  }
}
```

One sender means the secret is read in one place, the failure handling is written once, and adding the tenth event is a one-line call.

### 3. Call it from the moments that already exist

Look for these before writing anything new:

- **Inbound webhook handlers** the project already has — Stripe, GitHub, a payment provider. A `checkout.session.completed` handler is a new-customer event with no new plumbing.
- **Background jobs and queue workers** — the nightly import finished, the batch failed.
- **Deploy and CI pipelines** — a shell step calling `curl` is enough; no code change at all.
- **Domain events the app already emits**, if it has an event bus — subscribe the sender rather than editing each call site.

Fire the event **after** the work has committed, not before: an event announcing an order that then rolls back is worse than no event.

### 4. Don't let the event break the caller

An event is a nice-to-have; the request that triggered it is not.

- **Never fail the caller** because the event failed. Catch, log, carry on — as in the sender above.
- **Set a timeout.** A hanging call to a notification service must not hold a checkout open.
- **Keep it off the critical path** — send it after the response, or from a job, if the framework makes that easy.
- **Do not retry into a duplicate.** A retried send that actually succeeded the first time shows the user the same event twice; see "Idempotency" below.

### 5. Verify it end to end

Send one by hand and watch it land on a dashboard:

```bash
curl -X POST "https://api.dashboardbase.com/webhook/events/$DASHBOARDBASE_WEBHOOK_ID" \
  -H "Content-Type: application/json" \
  -d "{\"secret\": \"$DASHBOARDBASE_WEBHOOK_SECRET\", \"message\": \"Test event from the API\", \"emoji\": \"tada\"}"
```

`200` and the event appears for anyone viewing a dashboard in that organization. `401` means the secret is wrong, `404` means the webhook ID is wrong — check both against the workspace page.

## Idempotency

Events are fire-and-forget; the same event sent twice will appear twice. If your trigger source can fire repeatedly (e.g. a retried CI job), dedupe upstream — there is no built-in deduplication.

## Common mistakes

- **Hardcoding the secret in client-side code.** The secret authenticates the *sender*; it must stay server-side.
- **Sending an event on every signal.** Reserve events for things humans care about in real time; routine logs belong in a Status or Table widget, not an event.
- **Long `message`.** Keep it under ~100 characters; events are designed for glance-able notifications.
- **Guessing the webhook ID.** An unknown ID returns `404` and a wrong secret returns `401` — neither is silent, so check the status code before assuming the platform lost the event.
- **Sending an event from a request that might still roll back.** Send after the work commits.
