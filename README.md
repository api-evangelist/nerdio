# Nerdio

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
