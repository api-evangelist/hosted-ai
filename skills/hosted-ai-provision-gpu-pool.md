---
name: Provision a GPU pool on a node
description: Add a node to a Hosted·ai GPUaaS cluster, discover its GPUs, create a pool, and assign GPUs to it.
api: https://docs.hosted.ai/admin-panel/gpuaas/gpuaas-infrastructure-api
operations:
  - "POST /gpuaas/node/add"
  - "GET /gpuaas/node/scan_gpus"
  - "GET /gpuaas/node/scan_gpus/status"
  - "POST /gpuaas/pool/create"
  - "POST /gpuaas/pool/{poolId}/add_gpus"
---

# Provision a GPU pool on a node

Operating instructions for an agent to stand up GPU capacity on the Hosted·ai
GPUaaS Infrastructure (Admin Panel) API.

## Prerequisites

- Base URL is per deployment: `https://USER_PANEL_URL/api`.
- Authenticate every request with a bearer API token:
  `Authorization: Bearer YOUR_API_KEY` (create in User Panel > Account Settings >
  Security > Manage API Tokens; optional IP allowlist). See
  `../authentication/hosted-ai-authentication.yml`.
- Errors are plain HTTP status codes (401 = bad/expired token, 403 = insufficient
  permission, 400 = malformed request). See `../errors/hosted-ai-problem-types.yml`.

## Steps

1. **Add the node.** `POST /gpuaas/node/add` with the node connection details.
   Optionally verify reachability first with `GET /gpuaas/node/test_conn`.
2. **Discover GPUs.** `GET /gpuaas/node/scan_gpus`, then poll
   `GET /gpuaas/node/scan_gpus/status` until the scan completes. (Use
   `GET /gpuaas/node/scan_npus` for NPUs.)
3. **Create a pool.** `POST /gpuaas/pool/create` for the target GPUaaS instance.
   Capture the returned `poolId`.
4. **Assign GPUs.** `POST /gpuaas/pool/{poolId}/add_gpus` with the discovered GPU
   IDs. Confirm with `GET /gpuaas/{gpuaasId}/pool/available_gpus` /
   `GET /gpuaas/pool/{poolId}`.

## Notes

- No idempotency-key contract is documented — do not blindly retry a `POST` that
  may have partially succeeded; re-read state first.
- Pagination is not documented for list endpoints.
