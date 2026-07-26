---
name: Create and work a Duvo queue of cases
description: Create a queue, load durable cases into it, list and search them, and drive each case to completion for high-volume operational work.
api: openapi/duvo-ai-openapi-original.json
operations: [createQueue, createCases, listCases, searchCases, getCase, listCaseRuns]
---

# Create and work a Duvo queue of cases

Use this for high-volume, repeatable operational work — purchase orders, invoice matching, deductions, inventory exceptions — where each unit of work is a durable case an agent or specialist worker processes.

## Preconditions
- Provisioned workspace + workspace bearer token (`Authorization: Bearer <token>`).
- Scopes: `queues:write`, `runs:read` (see `scopes/duvo-ai-scopes.yml`).
- Include `Idempotency-Key` on `createQueue` / `createCases` to keep retries safe.

## Steps
1. **Create the queue** — `createQueue` (`POST /v2/teams/{teamId}/queues`). Capture `queue_id`.
2. **Load cases** — `createCases` (`POST /v2/queues/{queue_id}/cases`) to add durable cases (one per unit of work).
3. **List / search** — `listCases` (`GET /v2/queues/{queue_id}/cases`) and `searchCases` (`POST /v2/queues/{queue_id}/cases/search`) to triage.
4. **Inspect a case** — `getCase` (`GET /v2/cases/{case_id}`) and `listCaseRuns` (`GET /v2/cases/{case_id}/runs`) to see the runs that processed it.

## Rules
- Errors use the `{ error, message }` envelope; a `409` may carry only `{ error }`.
- Bulk operations exist for scale (`bulkReprocessCases`, `bulkRetryCases`, `bulkUpdateCaseStatus`) — prefer them over per-case loops.
- Human approvals and audit trails cannot be bypassed; route sensitive writes through approval gates.
