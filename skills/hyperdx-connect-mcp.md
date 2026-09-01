---
name: hyperdx-connect-mcp
description: Connect an agent to the HyperDX (ClickStack) MCP server and know which of its 27 tools do something the REST API cannot.
api: hyperdx:hyperdx-mcp-server
operations: []
generated: '2026-08-27'
method: generated
source: https://github.com/hyperdxio/hyperdx/blob/main/MCP.md
---

# Connect to the HyperDX MCP server

## Where it is

`{your-hyperdx-url}/api/mcp` — Streamable HTTP transport, Bearer authentication.

The endpoint is **instance-scoped**, not a vendor-hosted SaaS URL. HyperDX serves it from whatever
host runs the HyperDX app: your self-hosted deployment, or ClickStack in ClickHouse Cloud. The
vendor's own worked example is `http://localhost:8080/api/mcp`.

**HyperDX Cloud v1 (hyperdx.io) does not support the MCP server.** The vendor states this
directly, and a POST to `https://api.hyperdx.io/mcp` returns 404. If you are on v1 Cloud, this
skill does not apply to you — use the REST API.

## Credential

A **Personal API access key**, from the HyperDX UI under Team Settings → API keys → Personal API
access key. It can be rotated as of release 2.36.0 (2026-08-21); on older deployments it is fixed
for the life of the account.

The key has no scopes. An agent given a key to run `clickstack_search` can also run
`clickstack_delete_dashboard`. Scope the trust, not the credential — the credential cannot be
narrowed.

## Connecting

```bash
claude mcp add --transport http clickstack <your-hyperdx-url>/api/mcp \
  --header "Authorization: Bearer <your-personal-access-key>"
```

Codex CLI reads the token from an env var instead:

```bash
export CLICKSTACK_ACCESS_KEY="<your-personal-access-key>"
codex mcp add clickstack --url <your-hyperdx-url>/api/mcp \
  --bearer-token-env-var CLICKSTACK_ACCESS_KEY
```

Cursor and OpenCode take a `url` plus an `Authorization` header in their config files. Any client
supporting Streamable HTTP will work.

## What to reach for

Nineteen of the 27 tools are wrappers over documented REST operations. Eight are not, and those
are the reason to use MCP at all:

| Tool | Why it has no REST equivalent |
|---|---|
| `clickstack_list_metrics` | No REST operation enumerates metric names on a source |
| `clickstack_describe_metric` | No REST per-metric kind/unit/attribute drill-down |
| `clickstack_event_patterns` | Drain log-pattern clustering, server-side only |
| `clickstack_event_deltas` | Ranks properties by how two row groups' distributions differ |
| `clickstack_emerging_signals` | Two-window novelty diff — what patterns are NEW or GONE |
| `clickstack_sql` | Raw ClickHouse SQL; `/api/v2` has no raw-SQL operation |
| `clickstack_trace_waterfall` | Span tree with correlated logs |
| `clickstack_trace_top_time_consuming_operations` | Cumulative child-operation aggregation |

The full binding, including which REST operations back each of the other nineteen tools and which
twelve REST operations have no tool at all, is in `mcp/hyperdx-tool-crosswalk.yml`.

## What MCP cannot do

No tool covers team administration (invite, remove, list members) or ClickHouse connection writes,
and there is no delete tool for alerts or saved searches. Those are REST-only.

## Destructive tools

`clickstack_delete_source`, `clickstack_delete_dashboard` and `clickstack_delete_webhook` are all
permanent, with no undo and no recovery window. The dashboard delete also removes the alerts
attached to it. `clickstack_delete_webhook` is blocked while alerts still reference it — the only
pre-destructive guard on the surface.
