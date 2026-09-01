---
name: hyperdx-manage-dashboards
description: Build, validate and update a HyperDX dashboard over the API without breaking the alerts attached to its tiles.
api: hyperdx:hyperdx-external-api
operations:
  - listSources
  - listDashboards
  - getDashboard
  - validateDashboard
  - createDashboard
  - updateDashboard
  - deleteDashboard
  - queryChartSeries
generated: '2026-08-27'
method: generated
source: openapi/hyperdx-external-api-openapi.json
---

# Manage HyperDX dashboards

## Rehearse before you write

This is the one place in the HyperDX API that offers a dry run. `validateDashboard`
(`POST /api/v2/dashboards/validate`) checks a dashboard payload without persisting it. Use it on
every generated payload before `createDashboard` or `updateDashboard` — tile configs are a deep
discriminated union (chart kind × builder-vs-raw-SQL) and are easy to get subtly wrong.

If the MCP server is available, `clickstack_query_tile` and `clickstack_query_tiles` go one better
and actually execute the tiles' queries so you can see whether they return anything.

## Steps

1. `listSources` — every tile config references a source; get the ids first.
2. Compose the dashboard: `name`, `tiles`, optional `tags`, `filters`, `containers`.
   Each tile carries a position (`x`, `y`, `w`, `h`) and a `config`.
3. `validateDashboard` with the payload. Fix what it rejects.
4. `createDashboard` (`POST /api/v2/dashboards`), or `updateDashboard`
   (`PUT /api/v2/dashboards/{id}`) for an existing one.
5. `getDashboard` (`GET /api/v2/dashboards/{id}`) to read the persisted state back.

## Things that will bite you

- **`deleteDashboard` cascades to attached alerts.** It is permanent for both. If you are
  restructuring, `updateDashboard` — do not delete and recreate, because the alerts do not come
  back with the new dashboard.
- **No idempotency key.** A retried `createDashboard` creates a second dashboard. List first.
- **Series caps changed in 2.34.0.** The per-tile series limit is now a three-state value: omit it
  for the default cap, `0` for unlimited, or a positive N for the top N. Clients written before
  2026-08-07 that omitted the field to mean "all series" now silently get the default cap. Send
  `0` if you mean all.
- **Formulas landed in 2.36.0.** Line, stacked bar, table and number builder tiles accept
  `formulas` and `showOperandSeries`. Expressions are validated on write; formulas combined with
  `asRatio`, several formulas on a number tile, and unknown series refs are all rejected.
- **Ids carry no type prefix.** A dashboard id and a source id look identical. Track which
  collection an id came from; you cannot recover it from the id.

## Pagination

Some list operations take `limit` and `offset` and return a `meta` envelope with `total`, `limit`
and `offset`. Others return the whole collection unpaged. Check for `meta` rather than assuming.
