---
name: hyperdx-configure-source
description: Point HyperDX at a ClickHouse table and make it queryable — connection, source, and the OpenTelemetry field mapping that makes correlation work.
api: hyperdx:hyperdx-external-api
operations:
  - listConnections
  - createConnection
  - getConnection
  - updateConnection
  - listSources
  - getSource
  - createSource
  - updateSource
  - deleteSource
generated: '2026-08-27'
method: generated
source: openapi/hyperdx-external-api-openapi.json
---

# Configure a HyperDX data source

HyperDX stores almost no telemetry of its own. A Source is a saved description of how to read
someone else's ClickHouse table. Getting the description right is the whole job.

## Steps

1. **Connection.** `listConnections` (`GET /api/v2/connections`). If the ClickHouse server is not
   registered, `createConnection` (`POST /api/v2/connections`) with `name`, `host`, `username` and
   credentials. `isPrometheusEndpoint` marks a Prometheus-compatible endpoint rather than a
   ClickHouse server.

2. **Pick the source kind.** `log`, `trace`, `metric`, `session` or `promql`. The kind determines
   which expression fields the source requires — they are different schema variants, not one
   schema with optional fields.

3. **Map the OpenTelemetry fields.** This is where the value is. Each variant declares SQL
   expressions that project the table onto the OTel model:
   - all kinds: `timestampValueExpression`, `from`, `connection`, `querySettings`
   - log: `serviceNameExpression`, `severityTextExpression`, `bodyExpression`,
     `eventAttributesExpression`, `resourceAttributesExpression`, `traceIdExpression`,
     `spanIdExpression`
   - trace: `durationExpression`, `durationPrecision`, `parentSpanIdExpression`,
     `spanNameExpression`, `spanKindExpression`, `statusCodeExpression`, `statusMessageExpression`
   - metric: `metricTables` keyed by OTel metric kind, `resourceAttributesExpression`

4. **Wire the correlation graph.** This is the step people skip, and it is the one that makes
   HyperDX feel like HyperDX. Sources point at each other by id, in both directions:
   - `LogSource.traceSourceId` and `LogSource.metricSourceId`
   - `TraceSource.logSourceId`, `TraceSource.sessionSourceId`, `TraceSource.metricSourceId`
   - `MetricSource.logSourceId`
   - `SessionSource.traceSourceId`

   Without these, "jump from this log line to its trace, and from that span to the session replay"
   does not work. Create the sources first, then `updateSource` each one to fill in the ids of the
   others.

5. **Verify.** `getSource` (`GET /api/v2/sources/{id}`), then run a bounded `searchEvents` against
   it to confirm rows actually come back with the fields you mapped.

## Warnings

- `deleteSource` is permanent, with no restore. Saved searches referencing it via `sourceId`, and
  the alerts on those saved searches, will be pointing at nothing.
- No idempotency key: a retried `createSource` creates a duplicate source over the same table.
- If MCP is available, `clickstack_describe_source` returns the column schema, attribute keys,
  sampled low-cardinality values and a round-trippable `config` block — strictly more than
  `getSource` gives you, and the fastest way to clone a working source (read the config, drop the
  `id`, pass it to `clickstack_save_source`).
