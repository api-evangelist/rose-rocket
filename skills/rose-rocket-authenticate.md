---
name: rose-rocket-authenticate
description: >-
  Obtain and refresh an OAuth 2.0 access token for the Rose Rocket Platform Model API,
  by authorization code (acting for a user) or by client-credentials service account
  (machine to machine).
api: Rose Rocket Platform Model API
authorization_server: https://a.roserocket.com/
operations: []
generated: '2026-08-26'
method: generated
source: >-
  https://roserocket.readme.io/docs/rose-rocket-api-oauth-20-authentication-guide,
  https://roserocket.readme.io/docs/application-lifecycle-management, and the live
  discovery document at https://a.roserocket.com/.well-known/openid-configuration
  (HTTP 200, probed 2026-08-26). Full profile: authentication/rose-rocket-authentication.yml.
---

# Authenticate against Rose Rocket

Every Platform Model API request carries `Authorization: Bearer <access_token>`. The
published OpenAPI declares **no** security scheme, so do not expect a generated client to
attach one — it will not.

## Getting credentials is not self-serve

There is no developer signup. Existing Rose Rocket customers get API access through their
account representative; independent software vendors go through the Partnership team. Once
you have Developer Portal access, create the application in-product at
**Settings → API Settings → Applications → OAuth Applications → Create New Application**,
which yields a `client_id`, a `client_secret` and a redirect-URI list.

## Path A — authorization code (acting for a user)

1. Redirect the user to:

   ```
   https://a.roserocket.com/authorize
     ?audience=https://roserocket.com
     &scope=offline_access%20email%20profile
     &response_type=code
     &client_id={CLIENT_ID}
     &redirect_uri={REDIRECT_URI}
     &state={csrf_value}
   ```

   `audience` is mandatory and is always `https://roserocket.com`. PKCE (`S256`) is
   supported by the authorization server.

2. Exchange the returned `code`:

   ```
   POST https://a.roserocket.com/oauth/token
   Content-Type: application/x-www-form-urlencoded

   grant_type=authorization_code&client_id=…&client_secret=…&code=…&redirect_uri=…
   ```

## Path B — client credentials (service account)

Create the OAuth application, then create a Service Account user under it. The account
defaults to the **Manager** role.

```
POST https://a.roserocket.com/oauth/token
Content-Type: application/json

{ "grant_type": "client_credentials",
  "client_id": "…", "client_secret": "…",
  "audience": "https://roserocket.com",
  "org_id": "…", "user_id": "<service_account_user_id>" }
```

`org_id` and `user_id` are Rose Rocket-specific additions to the standard grant. Omitting
them will not get you a usable token.

## Refreshing

Token lifetime is deliberately variable — the guide warns sessions "may expire sooner than
expected". **Treat a single `401` as routine**, not as a failure:

```
POST https://a.roserocket.com/oauth/token
grant_type=refresh_token&refresh_token=…&client_id=…&client_secret=…&org_id=…
```

Refresh once, retry the call once, and only then surface an error. You must have requested
`offline_access` to hold a refresh token at all.

## What the scopes do and do not do

`offline_access`, `email` and `profile` are **OIDC identity scopes**. None of them grants
any API permission. Authorization is decided server-side by the **role** your user or
service account holds, against a per-object and per-field permission matrix set in the
product. You cannot read your effective permissions from the token, and no endpoint
enumerates them — you find out by receiving a `403`, which is not retryable and needs an
administrator to fix.

The only named permission the contract exposes is `userGroupResource`, with `viewer` and
`editor` levels, on the five user-group operations.

## Credential hygiene

- Rotating a client secret invalidates the previous one **immediately**, with no overlap
  window. Deploy the new secret before you rotate.
- Deleting an OAuth application **cannot be undone** and revokes its access at once.
