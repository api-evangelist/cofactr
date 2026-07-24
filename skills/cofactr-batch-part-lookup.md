---
name: Batch-resolve a BOM against Cofactr
description: Resolve many parts in one request against the Cofactr Knowledge Graph to enrich a bill of materials.
api: openapi/cofactr-knowledge-graph-openapi-original.json
operations: [batchProducts]
---

# Batch-resolve a BOM against Cofactr

Enrich a whole bill of materials (BOM) in a single call using the batch endpoint.

## Auth
- `X-CLIENT-ID: <your client id>`
- `X-API-KEY: <your api key>`

Base URL: `https://graph.cofactr.com`

## Steps
1. Assemble a `ProductsBatch` request body: a list of product queries (each mirroring
   the `listProducts` / `getProduct` parameters — `q`, `search_strategy`, `fields`, `schema`).
2. Call `batchProducts` (`POST /batch/products/`).
3. Read the `ProductBatchResponse` — each member returns a `BatchMemberResult` so you can
   map results back to your BOM lines individually.

## Rules
- Prefer batch over N single calls to stay under the concurrency ceiling.
- Choose the response `schema` (e.g. product-v0 / product-v1) deliberately — larger schemas cost more metered usage.
- Handle per-member results independently; a batch can partially succeed.
- Errors on the request as a whole use the `{ "detail": "..." }` envelope. See errors/cofactr-problem-types.yml.
