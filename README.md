# HyperDX

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

HyperDX is an open-source observability platform that unifies logs, metrics, traces, session replay and errors on top of ClickHouse and OpenTelemetry, giving engineers one place to search raw events, correlate a log line to its trace and that span to a session replay, build dashboards and run threshold alerts. It ships as an MIT-licensed self-hostable stack (the app, a Node.js API, an opinionated OpenTelemetry collector distribution and ClickHouse), as HyperDX Cloud at hyperdx.io, and as the user interface of ClickStack after ClickHouse acquired the company in March 2025. Its developer surface is a REST management API for dashboards, alerts, sources, saved searches, connections and webhooks, plus a first-party MCP server and an agent-friendly terminal CLI.

**Areas:** Company · Observability · Monitoring · Logging · Tracing · Metrics · OpenTelemetry · ClickHouse · Open Source · Alerting · Dashboards · Session Replay

## HyperDX on APIs.io

Everything profiled for HyperDX is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/hyperdx/](https://apis.io/providers/hyperdx/)** — or go straight to one of the APIs or properties below.

### APIs (11)

- [HyperDX Cloud API (v1)](https://apis.io/apis/hyperdx/hyperdx-cloud-api-v1/) — The documented v1 API on HyperDX Cloud, covering alerts, dashboards and chart series queries at api.hyperdx.io under /api/v1, authenticated with a Bearer…
- [HyperDX (ClickStack) MCP Server](https://apis.io/apis/hyperdx/hyperdx-mcp-server/) — First-party Model Context Protocol server shipped inside the HyperDX API package, served at /api/mcp on your instance over Streamable HTTP with Bearer…
- [HyperDX Alerts API](https://apis.io/apis/hyperdx/hyperdx-alerts-api/) — Endpoints for managing monitoring alerts
- [HyperDX Charts API](https://apis.io/apis/hyperdx/hyperdx-charts-api/) — Endpoints for querying chart data
- [HyperDX Connections API](https://apis.io/apis/hyperdx/hyperdx-connections-api/) — Endpoints for managing ClickHouse connections
- [HyperDX Dashboards API](https://apis.io/apis/hyperdx/hyperdx-dashboards-api/) — Endpoints for managing dashboards and their visualizations
- [HyperDX Saved Searches API](https://apis.io/apis/hyperdx/hyperdx-saved-searches-api/) — The Saved Searches API from HyperDX — 2 operation(s) for saved searches.
- [HyperDX Search API](https://apis.io/apis/hyperdx/hyperdx-search-api/) — Endpoints for querying raw data from log and trace sources
- [HyperDX Sources API](https://apis.io/apis/hyperdx/hyperdx-sources-api/) — Endpoints for managing data sources
- [HyperDX Team API](https://apis.io/apis/hyperdx/hyperdx-team-api/) — The Team API from HyperDX — 6 operation(s) for team.
- [HyperDX Webhooks API](https://apis.io/apis/hyperdx/hyperdx-webhooks-api/) — Endpoints for managing webhooks

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Hyperdx Webhooks](https://apis.io/asyncapis/hyperdx/hyperdx-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [HyperDX (ClickStack) MCP Server](https://apis.io/servers/hyperdx/hyperdx-clickstack-mcp-server/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Hyperdx External Api Overlay](https://apis.io/overlays/hyperdx/hyperdx-external-api-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [HyperDX — hyperdx rules](https://apis.io/rules/hyperdx/hyperdx-hyperdx-spectral-rules/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Hyperdx Authentication](https://apis.io/security/hyperdx/hyperdx-authentication/)
- [Hyperdx Domain Security](https://apis.io/security/hyperdx/hyperdx-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Hyperdx Plans Pricing](https://apis.io/plans/hyperdx/hyperdx-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Hyperdx Rate Limits](https://apis.io/rate-limits/hyperdx/hyperdx-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.hyperdx.io/)
- [Documentation](https://www.hyperdx.io/docs)
- [Getting started](https://www.hyperdx.io/docs/install)
- [API reference](https://www.hyperdx.io/docs/api/alerts)
- [Pricing](https://www.hyperdx.io/pricing)
- [Sign up](https://www.hyperdx.io/register)
- [Support](https://www.hyperdx.io/discord)
- [Blog](https://www.hyperdx.io/blog)
- [Terms of service](https://www.hyperdx.io/terms/service)
- [Privacy policy](https://www.hyperdx.io/terms/privacy)
- [GitHub](https://github.com/hyperdxio)
- [Status](https://hyperdx.statuspage.io/)

## API Evangelist rating

**Kin Score 68.1** — exemplar · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 68.3 |
| Contract quality | 59.9 |
| Contract governance | 50.0 |
| Operational transparency | 63.2 |
| Developer ergonomics | 80.4 |
| Access clarity | 76.3 |

**Agent readiness 37.7** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | templated |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | derived (partial credit) |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | partial |
| Operation Distinctiveness | distinct |

**Regulatory alignment 24.5** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/hyperdx/](https://apis.io/providers/hyperdx/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for HyperDX: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/hyperdx/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
