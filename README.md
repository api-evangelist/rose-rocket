# Rose Rocket

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Rose Rocket is a Toronto-based transportation management system (TMS) for freight brokers, carriers and 3PLs, sold as an AI-native, configurable operations platform rather than an off-the-shelf template. The product covers order intake, dispatch, tendering, tracking, documents, invoicing and billing, with no-code custom fields, boards and workflows, in-product AI agents (TED, Rosie, Rocky) and role-based access control down to the widget. Its developer surface is the Platform Model API, a single generic object-CRUD REST API served at network.roserocket.com/api/v2/platformModel that reads and writes every domain object — customer, order, task, stop, manifest, commodity, partner, quote, invoice, bill, asset, tag, contact, document — through one set of /objects operations keyed by objectKey, plus user groups, boards, events and webhook destinations. Authentication is OAuth 2.0 (authorization code or client-credentials service account) against an Auth0-hosted authorization server at a.roserocket.com. API access is not self-serve: credentials come from an account representative or the partnership team, and the pricing page lists "unlimited API access" as an Enterprise tier feature.

**Areas:** Transportation · Logistics · Freight · Trucking · Transportation Management System · Supply Chain · Dispatch · Webhook · Authentication · Canada

## Rose Rocket on APIs.io

Everything profiled for Rose Rocket is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/rose-rocket/](https://apis.io/providers/rose-rocket/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [Rose Rocket Boards API](https://apis.io/apis/rose-rocket/rose-rocket-boards-api/) — The Boards API from Rose Rocket — 1 operation(s) for boards.
- [Rose Rocket Events API](https://apis.io/apis/rose-rocket/rose-rocket-events-api/) — The Events API from Rose Rocket — 1 operation(s) for events.
- [Rose Rocket Object Records API](https://apis.io/apis/rose-rocket/rose-rocket-object-records-api/) — The Object Records API from Rose Rocket — 6 operation(s) for object records.
- [Rose Rocket User Groups API](https://apis.io/apis/rose-rocket/rose-rocket-user-groups-api/) — The User Groups API from Rose Rocket — 3 operation(s) for user groups.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Rose Rocket Webhooks](https://apis.io/asyncapis/rose-rocket/rose-rocket-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [rose-rocket-mcp.yml](https://apis.io/servers/rose-rocket/rose-rocket-mcp-yml/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Rose Rocket Platform Model Api Overlay](https://apis.io/overlays/rose-rocket/rose-rocket-platform-model-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Rose Rocket Authentication](https://apis.io/security/rose-rocket/rose-rocket-authentication/)
- [Rose Rocket Domain Security](https://apis.io/security/rose-rocket/rose-rocket-domain-security/)
- [Rose Rocket Vulnerability Disclosure](https://apis.io/security/rose-rocket/rose-rocket-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Rose Rocket Scopes](https://apis.io/scopes/rose-rocket/rose-rocket-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Rose Rocket Plans Pricing](https://apis.io/plans/rose-rocket/rose-rocket-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Rose Rocket Rate Limits](https://apis.io/rate-limits/rose-rocket/rose-rocket-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.roserocket.com/)
- [Documentation](https://roserocket.readme.io/docs/getting-started)
- [Getting started](https://roserocket.readme.io/docs/getting-started)
- [API reference](https://roserocket.readme.io/reference)
- [Pricing](https://www.roserocket.com/pricing)
- [Support](https://help.roserocket.com/)
- [Changelog](https://roserocket.readme.io/changelog)
- [Blog](https://www.roserocket.com/blog)
- [Terms of service](https://www.roserocket.com/terms)
- [Privacy policy](https://www.roserocket.com/privacy)
- [GitHub](https://github.com/RoseRocketSDK)
- [Status](https://status.roserocket.com/)

## API Evangelist rating

**Kin Score 60.5** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 48.7 |
| Contract governance | 18.2 |
| Operational transparency | 44.7 |
| Developer ergonomics | 54.2 |
| Access clarity | 67.1 |

**Agent readiness 42.0** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/rose-rocket/](https://apis.io/providers/rose-rocket/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Rose Rocket: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/rose-rocket/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
