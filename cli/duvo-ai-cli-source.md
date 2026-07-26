# Duvo CLI

Duvo publishes an official command-line interface on the public npm registry as `@duvoai/cli`. The CLI lets agents, evaluators, and developers script Duvo runs, cases, approvals, files, and inspections without building a custom API client from scratch.

## Install

```bash
npm install -g @duvoai/cli
duvo --version
duvo --help
```

- Package: [@duvoai/cli](https://www.npmjs.com/package/@duvoai/cli)
- Binary: `duvo`
- Runtime: Node.js 22.22 or later
- Distribution: npm (`npm`, `pnpm`, `yarn`, `bun`, `npx` are all supported).

Alternate install commands:

```bash
pnpm add -g @duvoai/cli
yarn global add @duvoai/cli
npx @duvoai/cli --help
```

## Authentication

The CLI stores a workspace API key in a named profile. Configure once with `duvo login`, or set the `DUVO_API_KEY` environment variable for non-interactive agent use. Use `DUVO_PROFILE` to select a named profile and `DUVO_API_BASE_URL` to override the API host. See [/auth.md](/auth.md) for the full authentication contract.

```bash
duvo login
export DUVO_API_KEY="<workspace-api-key>"
export DUVO_PROFILE="<profile-name>"
duvo whoami
```

## Common Commands

```bash
duvo runs start --agent <agent-id> --json
duvo runs get <run-id>
duvo runs messages <run-id>
duvo runs respond <run-id> --approve
duvo cases list --queue <queue-id>
duvo cases create --queue <queue-id> --title "case title"
duvo cases create --queue <queue-id> --from-file cases.json
duvo sandboxes create
duvo api GET /v2/runs/<run-id>
```

## When To Use The CLI

- Quick evaluation of a Duvo workspace without writing API client code.
- Shell scripts, cron jobs, and CI/CD pipelines that need to start, inspect, or respond to runs.
- Local debugging of agents, queue cases, and approvals.
- Agent and operator workflows that prefer a stable command surface over raw HTTP.

## Constraints

- The CLI calls the same workspace-scoped API documented in [/api-reference.md](/api-reference.md) and [/openapi.json](/openapi.json); it cannot bypass workspace RBAC, SSO, policy gates, or human approvals.
- API access requires a provisioned Duvo workspace and a workspace-issued bearer token. See [/auth.md](/auth.md).
- The CLI is published as an ES module and requires a recent Node.js LTS release.

## Related Resources

- CLI documentation hub: https://docs.duvo.ai/cli
- Developer platform overview: [/developers.md](/developers.md)
- API reference: [/api-reference.md](/api-reference.md)
- OpenAPI specification: [/openapi.json](/openapi.json)
- Authentication guide: [/auth.md](/auth.md)
- MCP guide: [/mcp.md](/mcp.md)
- Webhooks guide: [/webhooks.md](/webhooks.md)
- Getting started: [/getting-started.md](/getting-started.md)