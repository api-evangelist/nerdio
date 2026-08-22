# Nerdio

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

Nerdio (Nerdio, Inc., founded 2017, Chicago) builds automation and cost-optimization software for Microsoft cloud desktop environments — Azure Virtual Desktop, Windows 365, Intune and Microsoft 365. Its two products are Nerdio Manager for Enterprise (NME) for IT departments and Nerdio Manager for MSP (NMM) for managed service providers.

- https://getnerdio.com/

## APIs

| API | Contract | Notes |
|---|---|---|
| Nerdio Manager Distributor API | [OpenAPI 3.0.1](openapi/nerdio-distributor-api-openapi.json) | Public host `https://nmm-distributor-api.nerdio.net`; 9 operations; APIKey header. |
| Nerdio Manager for Enterprise REST API | not public | Customer-deployed; Swagger + Postman collection served from each install. OAuth2 client credentials via Microsoft Entra ID. |
| Nerdio Manager for MSP Partner API | not public | Customer-deployed; account-scoped `/rest-api/v1/accounts/{accountId}/`, with a v1-beta channel. |

## Artifacts

- `openapi/` — the Distributor API spec, harvested verbatim from `/swagger/v1/swagger.json`
- `overlays/` — API Evangelist enhancements over that spec (servers, security scheme, operationIds)
- `llms/` — Nerdio's own published `llms.txt`
- `packages/`, `cli/` — the first-party `NerdioManagerPowerShell` module (206 cmdlets, 231 models)
- `authentication/`, `conventions/`, `conformance/`, `errors/`, `lifecycle/`, `changelog/`, `data-model/`
- `asyncapi/` — the outbound notification webhook surface (no AsyncAPI published)
- `security/` — domain security probe, trust center, vulnerability disclosure probe
- `skills/`, `agentic-access/`, `mcp/` (candidate only — Nerdio operates no MCP server)
- `well-known/` — probe evidence; Nerdio publishes no `/.well-known/` document
