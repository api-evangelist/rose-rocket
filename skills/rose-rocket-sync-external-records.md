---
name: rose-rocket-sync-external-records
description: >-
  Synchronise records from an external system into Rose Rocket without creating
  duplicates, using upsert-by-external-id. This is the retry-safe write path and the
  one Rose Rocket's own documentation prescribes for integrations.
api: Rose Rocket Platform Model API
base_url: https://network.roserocket.com/api/v2/platformModel
operations:
  - PATCH /objects
  - PATCH /objects/{objectKey}/{externalId}/external
  - GET /objects/{objectKey}/{externalId}/external
generated: '2026-08-26'
method: generated
source: >-
  https://roserocket.readme.io/docs/upserts-and-external-ids and
  openapi/rose-rocket-platform-model-api.json
---

# Sync external records into Rose Rocket

Use this whenever a record already exists in another system (a TMS, an ERP, a
spreadsheet, your own database) and you need Rose Rocket to hold the same record.

## Why upsert and not create

`POST /objects` is **not** retry-safe. Rose Rocket states plainly that upsert "is not
supported with POST": a retried POST creates a second record. `PATCH /objects` keyed on
`externalId` is idempotent — run it a hundred times and you get one record.

There is no `Idempotency-Key` header on this API. `externalId` is the only idempotency
mechanism it has.

## Before you start

- An OAuth 2.0 bearer token. See the authentication skill.
- An `externalId` field must exist on the object you are syncing. It is added through the
  Object Builder in-product; it is not created by the API.
- The `objectKey` of the target entity — `customer`, `order`, `task`, `commodity`,
  `manifest`, `partner`, `quote`, `invoice`, `bill`, `contact`, `address`, `asset`, `tag`.

## Steps

1. **Upsert the record.**

   ```
   PATCH https://network.roserocket.com/api/v2/platformModel/objects
   Content-Type: application/json
   Authorization: Bearer <access_token>

   {
     "objectKey": "commodity",
     "json": {
       "externalId": "<your-system-id>",
       "freightClass": "75"
     }
   }
   ```

   - `201 Created` — no record carried that `externalId`, so a new one was created.
   - `200 OK` — a record matched and was updated.

   Both responses return the full record, including the Rose Rocket `id` (a UUID) and an
   incremented `version`. Store the `id`; it is what every other operation takes.

2. **Read it back by your own key** when you did not store the id:

   ```
   GET /objects/{objectKey}/{externalId}/external
   ```

3. **Create connected objects in the same call** where it makes sense. A connected object
   included in the payload is created with its parent — an `order` payload containing a
   `task` object creates both.

## Rules

- **Only the fields you send are touched.** Send the whole record when you mean to replace
  it; use `PUT /objects/{recordId}` for a full update by Rose Rocket id.
- **Derived fields are read-only** and will not accept a value. `invoice.subTotal` is
  computed from its connected `financialLineItems`.
- **A 500 on an upsert does not mean a duplicate.** The docs are explicit that a 500 "is
  not specific to duplicate external IDs". Retry with backoff; the operation is idempotent
  so a retry is safe.
- **Errors come back as** `{"statusCode": n, "message": ..., "error": "..."}`. On a 400 the
  `message` is an **array** of validation strings, not a string.
- **A 403 is not retryable.** It means the role behind your token lacks permission on that
  object or field. Only an admin can change that.
