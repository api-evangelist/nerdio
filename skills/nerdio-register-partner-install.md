---
name: Register and manage a partner's Nerdio Manager installs
description: >-
  Operating instructions for a distributor agent working the Nerdio Manager
  Distributor API — claim an MSP partner's Nerdio Manager for MSP installs against
  an Azure subscription, then suspend, reactivate or cancel them as the commercial
  relationship changes.
api: openapi/nerdio-distributor-api-openapi.json
base_url: https://nmm-distributor-api.nerdio.net
operations:
  - GET /api-v{version}/checkhealth
  - GET /api-v{version}/installs
  - POST /api-v{version}/register
  - PUT /api-v{version}/suspend
  - PUT /api-v{version}/reactivate
  - DELETE /api-v{version}/cancel
generated: '2026-08-01'
method: generated
source: openapi/nerdio-distributor-api-openapi.json
---

# Register and manage a partner's Nerdio Manager installs

Use this skill when a distributor's portal needs to reflect an MSP partner's Nerdio
Manager for MSP (NMM) purchase, cancellation or payment state in Nerdio's licensing
system (the "Mothership"). Every operation below exists verbatim in
`openapi/nerdio-distributor-api-openapi.json`.

## Before you start

- **Auth.** Every call requires the `APIKey` request header. The key is issued by
  Nerdio on request to `nmm.support@getnerdio.com`, together with access to the
  Mothership portal. There is no OAuth flow and no token refresh on this API.
  See `authentication/nerdio-authentication.yml`.
- **Version.** The version is a path segment: `/api-v1/...`. Supplying a version the
  API does not recognise is a documented cause of `400 Bad request`.
- **Scope of a call.** `register`, `suspend`, `reactivate` and `cancel` all key on
  `SubscriptionID` (an Azure subscription GUID) and act on **every** install in that
  subscription — not on one install. Confirm the subscription with a human before
  any of the four.
- **No idempotency key.** These are state-setting calls (see
  `conventions/nerdio-conventions.yml`). Do not blind-retry a mutation; re-read with
  `GET /api-v{version}/installs` first.

## Steps

1. **Smoke-test connectivity.** `GET /api-v{version}/checkhealth`. A `200` confirms
   the key and the version segment are both good before you attempt a mutation.

2. **Read current state.** `GET /api-v{version}/installs` returns every NMM install
   you have already registered, via this API or via the Mothership portal. Use it to
   decide whether a register is actually needed.

3. **Claim the partner's installs.** `POST /api-v{version}/register` with
   `SubscriptionID` (required) and optionally `SendConfirmationEmail` to notify the
   partner that their install has been assigned to you.
   - `200` returns the `InstallID`(s). **Handle an array** — one Azure subscription
     may carry more than one NMM install, and Nerdio's documentation explicitly tells
     integrators to absorb multiple `InstallID`s.
   - If no matching install exists yet, Nerdio creates an empty shell record and
     returns its `InstallID`; the record binds when the MSP later runs the NMM
     install against that subscription. This is a success, not an error.
   - `409 Conflict` means another distributor has already claimed those installs.
     Do not retry — escalate to Nerdio support.
   - `422` means the `SubscriptionID` is not a valid GUID.

4. **Suspend for non-payment.** `PUT /api-v{version}/suspend` with `SubscriptionID`.
   This blocks the MSP from logging in to their installs. Nerdio documents that
   **end users are not affected** — their desktops keep running — so this is a
   commercial lever, not a service outage. Say so when you report the action.

5. **Reactivate.** `PUT /api-v{version}/reactivate` with `SubscriptionID` restores
   login once payment clears.

6. **Cancel.** `DELETE /api-v{version}/cancel` with `SubscriptionID` removes your
   distributor assignment **and suspends the installs**. It is not a clean
   detach — treat it as destructive and require explicit human confirmation.

## Error handling

Every operation declares the same status set. From
`errors/nerdio-problem-types.yml`:

| Status | Meaning | What to do |
|---|---|---|
| 400 | Invalid parameters — e.g. wrong version number | Fix the request; do not retry unchanged |
| 401 | The `APIKey` does not authorize this request | Stop; the key is wrong or revoked |
| 409 | Another distributor already registered these installs | Stop; escalate to Nerdio support |
| 422 | `SubscriptionID` is not a valid GUID | Fix the GUID |
| 500 | Documented as parameter-driven, not purely server-side | Re-check parameter values before retrying |

No response body schema is published, so parse defensively and never assume a field
exists.
