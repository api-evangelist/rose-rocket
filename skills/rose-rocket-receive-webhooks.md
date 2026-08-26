---
name: rose-rocket-receive-webhooks
description: >-
  Register a Rose Rocket webhook destination through the object API, handle the delivery
  envelope, and compensate for the fact that deliveries are unsigned and unauthenticated.
api: Rose Rocket Platform Model API
base_url: https://network.roserocket.com/api/v2/platformModel
operations:
  - POST /objects
  - GET /objects/{recordId}
  - GET /events
generated: '2026-08-26'
method: generated
source: >-
  https://roserocket.readme.io/docs/webhooks-2. Full catalog:
  asyncapi/rose-rocket-webhooks.yml.
---

# Receive Rose Rocket webhooks

## Register the destination

Webhooks are ordinary records. There is no dedicated webhook API — you create a
`webhookDestination` through the same `/objects` endpoint as everything else.

```
POST /objects
{ "objectKey": "webhookDestination",
  "json": {
    "name": "Orders to our warehouse system",
    "url": "https://hooks.example.com/rose-rocket/<unguessable-path>",
    "subscriptions": [
      { "objectKey": "webhookSubscription", "eventName": "Order Status Changed" }
    ]
  } }
```

The same thing can be done in-product under **Profile → Settings → API Settings → Webhooks**.

## What events exist

One is documented: **Order Status Changed**. Rose Rocket says the available events "vary
depending on the modules or features you have enabled", but publishes no catalog of the
others. Do not build against an event you have not seen delivered.

## The delivery envelope

```json
{
  "id": "…",          // unique id of this webhook event
  "type": "…",        // event type
  "refId": "…",       // the record the event is about
  "ownerId": "…",     // who triggered it
  "orgId": "…",
  "objectKey": "…",   // which entity refId is
  "createdAt": "…",   // UTC
  "json": { }         // event-specific; no published schema
}
```

The seven scalar fields are identical on every event. Only `json` varies, and its shape is
not documented — **do not depend on it**. Use `refId` + `objectKey` and re-read the record.

## Handle it safely

1. **Verify nothing, because you cannot.** There is no signature, no shared secret, no
   timestamp, and — in Rose Rocket's own words — no way to give the destination a client id
   and secret for Basic Auth. Your only defences are an unguessable callback path and step 2.
2. **Re-fetch before acting.** Treat the delivery as a hint, not as data:
   `GET /objects/{refId}?paths=…`. This closes the spoofing gap and the missing-schema gap
   at the same time.
3. **Return 2xx fast.** Any failure code triggers a retry: 3 attempts, 30 seconds apart.
   Delivery is **at-least-once**, so make your handler idempotent on the event `id`.
4. **Accept that deliveries can be lost.** If all retries fail, "delivery is not
   guaranteed". There is no dead-letter queue and no replay endpoint. Failed deliveries are
   visible for **7 days** in the UI's Webhook Receipts and nowhere in the API.

## Reconciling a gap

There is no org-wide event stream to catch up from. `GET /events` requires **both** a
`recordId` and an `objectKey`, so it can only tell you about a record you already suspect.
If a delivery is lost and you do not know which record it concerned, you must re-scan with
`POST /objects/search` against a board.
