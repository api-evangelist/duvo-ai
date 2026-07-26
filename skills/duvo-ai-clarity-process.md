---
name: Explore and snapshot a Duvo Clarity process
description: List captured Clarity processes, read a process and its evidence-backed snapshots, and generate a new snapshot (catalogue, improvement plan, or transformation proposal).
api: openapi/duvo-ai-openapi-original.json
operations: [listClarityProcesses, getClarityProcess, listClarityProcessSnapshots, generateClarityProcessSnapshot, getClarityProcessSnapshot]
---

# Explore and snapshot a Duvo Clarity process

Use this to turn reviewed process evidence into a shared catalogue, improvement plan, transformation roadmap, or SAP migration fact base.

## Preconditions
- Provisioned workspace + workspace bearer token (`Authorization: Bearer <token>`).
- Scope: `runs:read` and read access to the team's Clarity processes.

## Steps
1. **List processes** — `listClarityProcesses` (`GET /v2/teams/{teamId}/clarity-v2/processes`). Capture a `process_id`.
2. **Read the process** — `getClarityProcess` (`GET /v2/clarity-v2/processes/{process_id}`).
3. **List snapshots** — `listClarityProcessSnapshots` (`GET /v2/clarity-v2/processes/{process_id}/snapshots/{kind}`) where `kind` selects the deliverable family.
4. **Generate a snapshot** — `generateClarityProcessSnapshot` (`POST /v2/clarity-v2/processes/{process_id}/snapshots/{kind}/generate`). This is a long-running generation; poll.
5. **Read the result** — `getClarityProcessSnapshot` (`GET /v2/clarity-v2/processes/{process_id}/snapshots/{kind}/{snapshot_id}`). Use `stopClarityProcessSnapshot` to cancel.

## Rules
- Outputs are grounded in reviewed process evidence — do not present ungrounded claims as Duvo output.
- Errors use the `{ error, message }` envelope (see `errors/duvo-ai-problem-types.yml`).
- Include `Idempotency-Key` on generate calls so retries do not duplicate work.
