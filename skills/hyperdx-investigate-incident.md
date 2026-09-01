---
name: hyperdx-investigate-incident
description: Investigate a production incident in HyperDX — find the error spike, chart it, drill into the raw events, and hand back a narrowed hypothesis.
api: hyperdx:hyperdx-external-api
operations:
  - listSources
  - getSource
  - queryChartSeries
  - searchEvents
generated: '2026-08-27'
method: generated
source: openapi/hyperdx-external-api-openapi.json
---

# Investigate an incident in HyperDX

Read-only. Every operation here is a GET or a query POST; nothing in this skill writes.

## Before you start

- Base URL is your HyperDX instance, not `api.hyperdx.io`: `https://<your-instance>/api/v2`.
  `api.hyperdx.io` serves the v1 API only and returns 404 for every `/api/v2` path.
- Auth on every request: `Authorization: Bearer <personal API access key>`.
- 401 means the header is missing or wrong. 403 means the key is valid but the team or
  resource is out of reach — do not retry a 403.

## Steps

1. **Find the source.** `listSources` (`GET /api/v2/sources`) returns the catalog. Each source
   has a `kind` — `log`, `trace`, `metric`, `session` or `promql`. Pick the log or trace source
   that covers the service under investigation. If several look plausible, do not guess: call
   `getSource` (`GET /api/v2/sources/{id}`) and read `serviceNameExpression` and `from` to see
   which ClickHouse table it actually reads.

2. **Chart the shape of the problem.** `queryChartSeries`
   (`POST /api/v2/charts/series`) with a `granularity` so you get a time series rather than one
   number. Group by service or by whatever dimension you suspect. You are looking for when the
   line moves, not for a precise value.

3. **Narrow before you read rows.** Re-run `queryChartSeries` with a tighter time range around
   the inflection point and a different `groupBy`. Two or three passes here are far cheaper than
   pulling raw events over a wide window.

4. **Read the raw events.** `searchEvents` (`POST /api/v2/search`) against the same source with
   the narrowed window and a `where` clause. The row shape is determined by the source's SELECT
   expressions, so it is not predictable from the spec — read what comes back rather than
   assuming fields.

5. **Follow the correlation.** Sources cross-reference each other by id: a log source carries
   `traceSourceId` and `metricSourceId`, a trace source carries `logSourceId` and
   `sessionSourceId`. Use those to jump from a log line to its trace, or from a span to the
   session replay, instead of searching each source blind.

## If the MCP server is available

The instance also serves 27 MCP tools at `/api/mcp`. Several have no REST equivalent and are
better than anything above for this job: `clickstack_event_patterns` (Drain clustering over the
result set), `clickstack_emerging_signals` (what patterns are NEW or GONE versus a baseline
window), `clickstack_event_deltas` (which properties differ between two row groups), and
`clickstack_trace_waterfall`. If you have MCP access, start with `clickstack_emerging_signals` —
it answers "what changed" directly, which is the question step 2 only approximates.

## Errors

Error bodies are `application/json` with a single `message` string, conventionally prefixed with
an uppercase code (`"NOT_FOUND: Alert not found"`). There is no separate code field and no
published enumeration of prefixes. 400 on a query means the query failed to compile as often as
it means the body was malformed — read the message. 500 is safe to retry once on a read.
