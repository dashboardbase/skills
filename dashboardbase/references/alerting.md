# Alerting

> **Load this file when:** adding an alert to a widget endpoint, changing a threshold, or working out why an alert is not firing.

Alerting has **two sides, and only one of them is code**:

1. **Your endpoint** returns an `alert` object in the response envelope when the metric crosses a threshold.
2. **The user enables alerts on that widget** in the dashboard editor.

Until side 2 is done, side 1 is inert — the endpoint keeps returning `alert` and no notification is ever sent. This is the single most common reason "the alert doesn't work", so when you add alerting to an endpoint, always tell the user to switch it on for that widget and say so in your handover.

## Side 1 — returning an alert

`alert` is an envelope key, alongside `title`, `actions` and `data` — never inside `data`. It works identically on every widget type.

```json
{
  "title": "Error rate",
  "data": {
    "header": {
      "title": "7.4%",
      "subtitle": "Last 24 hours",
      "badge": { "text": "+2.1pp", "icon": "ArrowUp", "color": "Danger" }
    }
  },
  "alert": {
    "active": true,
    "level": "critical",
    "message": "Error rate 7.4% is above the 5% threshold"
  }
}
```

- `active` (required boolean) — `true` raises the alert, `false` clears it.
- `level` (required) — one of `info`, `success`, `warning`, or `critical`. These are **lowercase**, unlike every other enum in the contract.
- `message` (required, non-empty string) — what a human needs to read. Keep it under ~200 characters; longer messages are truncated.

**All three fields are required whenever `alert` is present.** To clear an alert you either omit the `alert` key entirely, or send `"active": false` *with* a `level` and a `message` — `{ "active": false }` on its own fails validation. When `active` is `false` the level and message are discarded, so any valid values will do.

Compute the threshold server-side, in the same handler that builds the payload:

```js
const errorRate = failed / total;
const breached = errorRate > 0.05;

res.json({
  data: { header: { title: `${(errorRate * 100).toFixed(1)}%`, subtitle: "Last 24 hours" } },
  alert: {
    active: breached,
    level: "critical",
    message: `Error rate ${(errorRate * 100).toFixed(1)}% is above the 5% threshold`
  }
});
```

Do not keep alert state in your own database. Dashboardbase stores whether the alert is currently active and only notifies on the transition — your endpoint just reports the current truth on every poll.

## Side 2 — enabling alerts on the widget

Alerts are enabled **per widget**, by the user, in the dashboard editor: open the dashboard, edit the widget, and turn alerts on for it. There is no way to enable alerting from the endpoint, from the setup file, or from a request — it is a deliberate opt-in, because an alert-enabled widget is polled on its own schedule and can send email, push and Slack notifications.

So the handover for a new alert reads:

> The endpoint now returns an `alert` when <metric> crosses <threshold>. To start getting notified, open the dashboard, edit the <name> widget and switch alerts on for it. The dashboard also has to be published — alerts are not polled on drafts.

## How alerting behaves

Worth knowing before you promise the user something the platform does not do:

- **Alerts are polled on their own ~5-minute schedule**, independently of the dashboard's `refreshInterval`. A `30m` dashboard still has its alert-enabled widgets checked every few minutes; a `1m` dashboard does not get alert checks every minute.
- **Only published dashboards are polled.** Draft dashboards and draft widgets are skipped entirely, so an alert added while the dashboard is still a draft will look dead until it is published.
- **Notifications fire on the inactive → active edge only.** An alert that stays active does not re-notify on every poll, and clearing an alert sends nothing. If the user needs a reminder while something is still broken, that is a pager's job, not this.
- **A failed poll leaves the previous state alone.** If your endpoint times out or returns something invalid, the alert is neither raised nor cleared — deliberately, so a brief outage cannot clear an active alert and then re-fire it on recovery.
- **Only alert metadata is stored** — active / level / message / last-checked. The widget's business data is never persisted.
- **Delivery is email, push and Slack**, each subject to the recipient's own notification settings and the organization's Slack integration. A member who has opted out of alert emails gets none, however critical the level.

## Writing an alert worth reading

- **Pick the level by what the reader should do.** `critical` — someone acts now. `warning` — look at it today. `info` / `success` — context, e.g. a recovery or a milestone.
- **Put the number and the threshold in the message.** "Error rate 7.4% is above the 5% threshold" beats "Error rate high" — the reader can decide without opening anything.
- **Add hysteresis if the metric sits near the line.** Raise at 5% but clear only below 4%, or require two consecutive breaching windows; otherwise a metric hovering at the threshold flips active/inactive and notifies on every upward crossing.
- **Alert on what a human would act on.** A widget whose alert is always active is noise, and its notification only ever fires once anyway.
- **Keep the payload useful while alerting.** The widget still renders `data`; leave the header showing the breaching number so the tile explains itself.

## Checklist

- [ ] `alert` is at the envelope level, with `active`, `level` and `message` all present.
- [ ] `level` is lowercase.
- [ ] The alert clears — verify the endpoint returns `active: false` (or no `alert`) when the metric is healthy.
- [ ] The response validates with `validate_widget_response`, or `data` validates against `assets/schemas/<widget>.json`.
- [ ] The user has enabled alerts on that widget in the editor.
- [ ] The dashboard is published, not a draft.
- [ ] The people who should hear about it have alert notifications switched on.
