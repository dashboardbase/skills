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

`{webhookId}` is the UUID created when you set up the webhook in Dashboardbase.

### Fields

- `secret` (required) — the shared secret. Events with a mismatched secret are discarded silently.
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

`200 OK` if the secret matches and the event was queued. `4xx` otherwise.

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

## Idempotency

Events are fire-and-forget; the same event sent twice will appear twice. If your trigger source can fire repeatedly (e.g. a retried CI job), dedupe upstream — there is no built-in deduplication.

## Common mistakes

- **Hardcoding the secret in client-side code.** The secret authenticates the *sender*; it must stay server-side.
- **Sending an event on every signal.** Reserve events for things humans care about in real time; routine logs belong in a Status or Table widget, not an event.
- **Long `message`.** Keep it under ~100 characters; events are designed for glance-able notifications.
- **Sending events before the user has configured a webhook.** Events with no matching webhook are dropped silently — verify the webhook exists during onboarding.
