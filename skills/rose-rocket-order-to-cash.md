---
name: rose-rocket-order-to-cash
description: >-
  Walk a freight order through Rose Rocket from customer to invoice — create the
  customer, create the order with its tasks and commodities, read the manifest and
  financial line items, and reach the invoice — using the generic object operations.
api: Rose Rocket Platform Model API
base_url: https://network.roserocket.com/api/v2/platformModel
operations:
  - POST /objects
  - GET /objects/{recordId}
  - PUT /objects/{recordId}
  - POST /objects/search
  - GET /objects/autocomplete
generated: '2026-08-26'
method: generated
source: >-
  https://roserocket.readme.io/docs/getting-started,
  https://roserocket.readme.io/docs/object-descriptions-and-operations,
  and the object reference pages for order, task, commodity, manifest, invoice and
  financial-line-items. Entity graph: data-model/rose-rocket-data-model.yml.
---

# Order to cash in Rose Rocket

Every step below uses the same endpoint. `objectKey` selects the entity.

## 1. The customer must exist first

Customer data is required before an order or a quote can reference it.

```
POST /objects
{ "objectKey": "customer",
  "json": { "name": "ABC Produce Co", "customerType": "broker", "defaultCurrency": "USD" } }
```

`customerType` is one of `Shipper`, `Broker`, `Receiver`, `Other`. Keep the returned `id`.
The human-readable `fullId` (e.g. `CUST-18`) is for people, not for API calls.

If the customer may already exist, use `GET /objects/autocomplete?objectKey=customer&searchTerm=…`
first, or upsert on `externalId` — see the sync skill.

## 2. Create the order

An order is the revenue-generating agreement and the container for the work.

```
POST /objects
{ "objectKey": "order",
  "json": { "customer": { "id": "<customerId>" },
            "tasks": [ { "name": "Pickup at origin" } ] } }
```

Connected objects passed inline are created with the parent, so tasks, stops and
commodities can go in the same call.

## 3. Read the order back with its graph

Connected objects are **omitted by default**. Name them:

```
GET /objects/{orderId}?paths=tasks,commodities,stops,sellQuote,buyQuotes,tags
```

The order's relationships: `customer` and `partner` (belongs_to), `tasks[]`, `tags[]`,
`csrs[]`, `accountManagers[]`, `commissionees[]` (users), `sellQuote` (the customer-facing
revenue quote) and `buyQuotes[]` (partner cost quotes).

## 4. Follow the money

Financial amounts do not live on the order. They live on `financialLineItem` records, each
of which belongs to whichever financial object it is posted against — `order`, `quote`,
`manifest`, `invoice`, `bill` — and carries `glAccount`, `taxRate` and a `copiedFrom` /
`copiedTo` lineage that traces a line from quoted through to invoiced.

```
GET /objects/{invoiceId}?paths=lineItems,customer,address,documents
```

`invoice.subTotal` is **read-only**, derived from `lineItems`. Do not try to write it.
The partner-side mirror of an invoice is a `bill`, connected to a `partner` and a `manifest`.

## 5. Search across a board

```
POST /objects/search
{ "boardId": "<boardId>", "objectKey": "order",
  "filters": [...], "filtersOperator": "and",
  "orderByPath": "createdAt", "orderByDirection": "desc", "limit": 50 }
```

`boardId` is **required**. A board is a saved view and also a permission boundary, so the
same records can look different through different boards.

**Pagination warning:** this operation takes `limit` and nothing else — no offset, no
cursor, no total count. A result set larger than `limit` cannot be walked. Narrow with
filters rather than paging.

## Cautions

- `DELETE /objects/{recordId}` and `POST /objects/bulk_delete` have **no reversal path** —
  no undo, no restore, no retention window is documented. Bulk delete returns `204` with no
  per-id result.
- Only `GET /events` supports real pagination (`limit`, `offset`, `bookmark`), and it needs
  both a `recordId` and an `objectKey`, so it is a per-record feed.
- No rate limits are published and no `429` is documented. Throttle yourself conservatively.
