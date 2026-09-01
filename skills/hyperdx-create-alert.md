---
name: hyperdx-create-alert
description: Create, update and route a HyperDX threshold alert — including the webhook destination it notifies.
api: hyperdx:hyperdx-external-api
operations:
  - listWebhooks
  - createWebhook
  - listSavedSearches
  - createSavedSearch
  - listAlerts
  - createAlert
  - getAlert
  - updateAlert
  - deleteAlert
generated: '2026-08-27'
method: generated
source: openapi/hyperdx-external-api-openapi.json
---

# Create a HyperDX alert

This skill writes. Read the warnings before you run it.

## Warnings

- **No idempotency.** There is no `Idempotency-Key` header on this API. If `createAlert` times out
  and you retry it, you will have two alerts. Before retrying any create, call the matching list
  operation and check whether the resource already exists.
- **No undo.** `deleteAlert` is permanent. There is no restore, no trash, no version history and
  no recovery window.
- **Deleting a dashboard deletes its alerts.** Alerts attached to a dashboard tile go with the
  dashboard.

## Steps

1. **Have a destination first.** `listWebhooks` (`GET /api/v2/webhooks`). If nothing suitable
   exists, `createWebhook` (`POST /api/v2/webhooks`) with `name`, `service` and `url`. `service`
   is one of `slack`, `incidentio` or `generic`. `generic` accepts an optional body template,
   headers and query params; `slack` rejects all three, because it posts a fixed payload.

2. **Decide what the alert watches.** An alert attaches to exactly one of two things, and the
   two field sets are mutually exclusive:
   - a dashboard tile — send `dashboardId` and `tileId`
   - a saved search — send `savedSearchId` and optionally `groupBy`

   Sending both is a validation error. If you need a saved search, `listSavedSearches` first, then
   `createSavedSearch` (`POST /api/v2/saved-searches`) with `sourceId`, `select` and `where`.

3. **Create the alert.** `createAlert` (`POST /api/v2/alerts`) with `threshold`,
   `thresholdType`, `interval`, `source` and `channels`. `channels` is an array of 1 to 10 entries,
   each `{ "type": "webhook", "webhookId": "<id>" }`; duplicates are rejected. Optional `name`,
   `message`, `note` and `numConsecutiveWindows`.

4. **Verify.** `getAlert` (`GET /api/v2/alerts/{id}`) and read the object back. Do not treat the
   201-shaped response as confirmation that the alert evaluates — read it back.

5. **Change rather than recreate.** `updateAlert` (`PUT /api/v2/alerts/{id}`) takes the same body
   as create. Prefer it over delete-then-create, because the delete half is the irreversible half.

## Cleaning up a webhook

`deleteWebhook` returns **409** while any alert still references it. That is the guard working,
not a transient error — detach the webhook from every alert first. A 409 on `updateWebhook` means
something else modified it concurrently: re-read the current state and reapply.

Note the update asymmetry on webhooks: on `PUT`, omitted readable fields (`description`, `body`)
are **cleared**, while omitted `headers`/`queryParams` are **preserved** — send an explicit `{}`
to clear those. If you change `url` or `service`, omitted headers and query params are cleared
rather than preserved, so stored secrets never follow a webhook to a new address.

## Cloud versus self-hosted

Alert channels on the open-source build are webhooks only. HyperDX Cloud additionally offers
email, Slack OAuth, PagerDuty and Opsgenie. Write against the channel set your deployment has.
