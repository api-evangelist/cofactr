---
name: Find an electronic component with Cofactr
description: Search the Cofactr Knowledge Graph for an electronic part and retrieve its details, offers, and pricing.
api: openapi/cofactr-knowledge-graph-openapi-original.json
operations: [listProducts, getProduct, autocompleteProducts]
---

# Find an electronic component with Cofactr

Use the Cofactr Knowledge Graph (Component Cloud) API to find a part and read its
distributor offers and pricing.

## Auth
Send both headers on every request (server-side only — never expose in front-end code):
- `X-CLIENT-ID: <your client id>`
- `X-API-KEY: <your api key>`

Base URL: `https://graph.cofactr.com`

## Steps
1. (Optional) Suggest matches as the user types with `autocompleteProducts`
   (`GET /products/autocompletions/?q=<partial>`).
2. Search with `listProducts` (`GET /products/`):
   - `q` = the manufacturer part number or search text.
   - `search_strategy` = one of `default`, `mpn_sku_mfr`, `mpn_exact`, `mpn_exact_mfr`
     (use `mpn_exact` when you have an exact MPN).
   - `fields` = comma-separated list to limit the response payload.
   - Page with `limit` + `page_token` (base64 cursor from the previous response).
3. Read a specific part with `getProduct` (`GET /products/{id}`) using the Cofactr part id.

## Rules
- Errors return `{ "detail": "..." }` with `application/json` (not RFC 9457). See errors/cofactr-problem-types.yml.
- A `429` means the monthly product quota or the concurrent-request ceiling was hit — back off. See rate-limits/cofactr-rate-limits.yml.
- Usage is metered per Part. Avoid `force_refresh=true` unless you truly need fresh distributor data — it can consume distributor rate limits.
