# Browserbase MCP server

Inspect existing sessions, approve creation only if needed, then request release of the returned session ID and inspect its final state. Actual browser driving requires a separate CDP client.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `BROWSERBASE_API_KEY`



Credential help: https://docs.browserbase.com/reference/api/openapi.v1.yaml

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call list_sessions '{"status":"RUNNING"}'
```

## Use with an MCP client

Launch `node /absolute/path/browserbase-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| list_sessions | Read | List sessions, optionally filtered by provider status. |
| get_session | Read | Inspect a session’s lifecycle metadata. Treat returned connection URLs and signing information as sensitive; no browser navigation is performed. |
| create_session | Explicit approval required | Create a billable short-lived session only after explicit approval; release it when done or wait for provider timeout. |
| release_session | Explicit approval required | Request early release of the chosen session after approval; inspect returned lifecycle state rather than assuming completion. |

## Concrete workflow

Inspect existing sessions, approve creation only if needed, then request release of the returned session ID and inspect its final state. Actual browser driving requires a separate CDP client.

1. list_sessions
2. create_session
3. release_session
4. get_session

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-browserbase-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/browserbase/).

## Provenance and maintenance

- https://docs.browserbase.com/reference/api/openapi.v1.yaml

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
