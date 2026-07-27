---
name: Inventory and monitor GPUaaS capacity
description: Enumerate regions, nodes and pools on Hosted·ai and read pool performance metrics.
api: https://docs.hosted.ai/admin-panel/gpuaas/gpuaas-infrastructure-api
operations:
  - "GET /gpuaas/list/regions"
  - "GET /gpuaas/node/list"
  - "GET /gpuaas/{gpuaasId}/pool/list"
  - "GET /gpuaas/pool/{poolId}/metrics"
  - "GET /gpuaas/node/{nodeId}/storage_metrics"
---

# Inventory and monitor GPUaaS capacity

Operating instructions for an agent to read Hosted·ai GPUaaS state through the
Admin Panel API (read-only).

## Prerequisites

- Base URL per deployment: `https://USER_PANEL_URL/api`.
- Bearer API token on every request (`Authorization: Bearer YOUR_API_KEY`). See
  `../authentication/hosted-ai-authentication.yml` and
  `../conventions/hosted-ai-conventions.yml`.

## Steps

1. **List regions.** `GET /gpuaas/list/regions` to scope the estate.
2. **List nodes.** `GET /gpuaas/node/list`; drill into any node with
   `GET /gpuaas/node/{nodeId}` and `GET /gpuaas/node/{nodeId}/storage_metrics`.
3. **List pools.** `GET /gpuaas/{gpuaasId}/pool/list` for the GPUaaS instance.
4. **Read metrics.** `GET /gpuaas/pool/{poolId}/metrics` for per-pool utilization.

## Notes

- When filtering by date, supply explicit times or a range: a date without a
  timestamp is set to midnight (00:00:00), so start == end returns zero results
  (see `../conventions/hosted-ai-conventions.yml`).
- A `401` means the token is invalid/expired; a `403` means it lacks permission
  for that resource.
