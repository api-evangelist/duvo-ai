---
name: Start and monitor a Duvo run
description: Start an agent-backed operational run in an approved workspace, stream its messages, answer any human-approval request, and stop it if needed.
api: openapi/duvo-ai-openapi-original.json
operations: [startRun, getRun, listRunMessages, respondToHumanRequest, stopRun]
---

# Start and monitor a Duvo run

Use this when a team needs an agent-backed operational run (e.g. invoice matching, a price change, a supplier-onboarding case) executed in an approved Duvo workspace.

## Preconditions
- A provisioned Duvo workspace and a workspace-issued bearer token. Public no-signup sandbox access is NOT available (see `pricing.md`).
- Send credentials as `Authorization: Bearer <token>`.
- Include an `Idempotency-Key` header on the start request so a retry does not create a duplicate run.

## Steps
1. **Start the run** — `startRun` (`POST /v2/teams/{teamId}/runs`). Supply the target agent and inputs. Capture the returned `run_id`.
2. **Poll status** — `getRun` (`GET /v2/runs/{run_id}`). Watch for terminal states; a `403` means insufficient scope, RBAC denial, or a pending human approval.
3. **Read output** — `listRunMessages` (`GET /v2/runs/{run_id}/messages`) to stream tool calls, files, and audit events.
4. **Answer approvals** — when a human-approval request appears, call `respondToHumanRequest` (`POST /v2/runs/{run_id}/human-requests/{request_id}/respond`) to approve, reject, or answer. Agents cannot bypass approval gates.
5. **Stop if needed** — `stopRun` (`POST /v2/runs/{run_id}/stop`) to halt a run.

## Rules
- Treat `401` as a bad/expired/revoked credential; `403` as insufficient scope or an approval requirement (`approval_required`).
- On `rate_limited`, honor `Retry-After` and retry with the same `Idempotency-Key`.
- Errors return `{ "error": "<code>", "message": "<text>" }` (see `errors/duvo-ai-problem-types.yml`).
- Required scopes: `runs:write`, `runs:read`, `approvals:write` (see `scopes/duvo-ai-scopes.yml`).
