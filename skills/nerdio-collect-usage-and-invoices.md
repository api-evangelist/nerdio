---
name: Collect Nerdio Manager usage and invoice data for billing
description: >-
  Operating instructions for pulling daily usage and monthly invoice data out of the
  Nerdio Manager Distributor API, including the invoice line-item vocabulary a
  distributor needs to reconcile a Nerdio invoice against its own billing run.
api: openapi/nerdio-distributor-api-openapi.json
base_url: https://nmm-distributor-api.nerdio.net
operations:
  - GET /api-v{version}/checkhealth
  - GET /api-v{version}/usage
  - GET /api-v{version}/invoices
  - GET /api-v{version}/invoices/grouped
generated: '2026-08-01'
method: generated
source: openapi/nerdio-distributor-api-openapi.json
---

# Collect Nerdio Manager usage and invoice data for billing

Use this skill for the monthly distributor billing run and for daily consumption
reporting. All four operations exist verbatim in
`openapi/nerdio-distributor-api-openapi.json`. All are reads — nothing here changes
a partner's state.

## Before you start

- **Auth.** `APIKey` request header on every call. See
  `authentication/nerdio-authentication.yml`.
- **Date format.** Period start and end dates must be `mm/dd/yyyy`. A malformed
  range is a documented `400`.
- **No pagination.** These collection endpoints declare no page or cursor
  parameters — scope the result set with the date range
  (`conventions/nerdio-conventions.yml`).
- **Ground truth.** Nerdio's guidance is to view the invoice in the Mothership
  portal first; the API response is designed to match that invoice exactly.

## Steps

1. **Smoke-test.** `GET /api-v{version}/checkhealth` before a batch run.

2. **Daily usage.** `GET /api-v{version}/usage` returns consumption information —
   this is the daily-cadence endpoint.

3. **Monthly invoice.** `GET /api-v{version}/invoices` for the period. Nerdio also
   sends a monthly invoice separately; the response is meant to reconcile against it
   line for line.

4. **Break it down.** `GET /api-v{version}/invoices/grouped` returns the same
   consumption grouped by partner and account — use this to allocate charges to the
   MSP and to its end customers.

5. **Interpret the line items.** Each row carries a `Type`. From
   `data-model/nerdio-data-model.yml`:
   - **AVD Users** — end users assigned an AVD desktop, averaged over the calendar
     month.
   - **Cloud PC-only Users** — end users assigned a Windows 365 Cloud PC but *not* an
     AVD desktop, averaged over the calendar month. A user with both is counted as an
     AVD user, not twice.
   - **Discount** — a cost reduction applied at account, partner (install) or
     distributor level.
   - **Price Control** — a floor or ceiling agreed with a partner (e.g. a $1,000/month
     minimum, or a $50,000/month cap). Apply it after summing usage, not per line.
   - **Upcharge** — charges for services other than AVD and Cloud PC licences, applied
     to the invoice overall rather than per install. Rare.

## Empty results are not errors

An invoice request for a period with no usage returns **HTTP 200 with an empty
body** — this is documented behaviour. Treat it as "no charges", never as a failure,
and never retry it as though it were transient.

## Error handling

Same uniform status set as the rest of the API — see
`errors/nerdio-problem-types.yml`. For this read-only flow the ones that matter are
`400` (bad date range or version), `401` (bad `APIKey`) and `422` (bad subscription
GUID). No response schema is published; parse defensively.
