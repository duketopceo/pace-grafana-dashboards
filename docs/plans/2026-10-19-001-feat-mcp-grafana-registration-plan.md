---
artifact_contract: ce-unified-plan/v1
product_contract_source: ce-plan user-directed
external_source: grafana/mcp-grafana (github.com/grafana/mcp-grafana) — 3.6k★, Apache-2.0, Go, pushed 2026-10-19
---

# mcp-grafana — dashboards become agent-queryable

## Goal Capsule

Register Grafana's official MCP server so agents can query dashboards, datasources, alerts, and incidents — closing the loop between this repo's dashboard JSON and the agents that debug with them. Small, official, low-risk.

## Product Contract

### Why

pace-grafana-dashboards is the dashboard catalog; `mcp-grafana` makes them callable from agents — "agent sees alert → reads dashboard state → correlates" without a human copy-pasting panels.

### Scope boundaries

- **In:** one MCP server registration (stdio `mcp-grafana` or HTTP per upstream's supported transport), **read-only Grafana service-account token**, catalog atom + harness config entries, verification.
- **Out:** dashboard provisioning changes; alerting config; any write-scoped token.

### Requirements

1. Service-account token scoped read-only (`Viewer` role minimum viable). Token lives in OmaSeal (`grafana/mcp-reader` or similar), never in the MCP config file.
2. Catalog atom in the luke-agents MCP map + entries in harness configs (matching the kurultai bridge pattern for secret resolution at spawn).
3. Verify: `tools/list` shows search/dashboard/datasource tools; one real query (`search dashboards`, read a panel's query) round-trips.
4. When the ContextForge gateway lands (luke-agents plan), this registers as an upstream there — no per-harness wiring survives that migration.

### Acceptance examples

- An agent calls "list dashboards" and gets this repo's deployed dashboards; can fetch one dashboard's panel queries.
- Token in omaseal; config contains only the resolver reference.

## Repo Touchpoints

- `MANIFEST.md` / docs note listing the MCP endpoint (this repo is dashboard JSON — the config lives in dotfiles/luke-agents, this doc records the linkage)

## Test Scenarios

1. `tools/list` through the server enumerates expected tools.
2. Read-only enforcement: any write call denied by Grafana RBAC.
3. Bad/missing token → fail closed, clear error, no token in logs.

## Risks / Honest Notes

- The only real risk is token scope — a write-capable Grafana token in an MCP config is a footgun. Viewer-only, in omaseal.
- Small enough that "plan" is generous — it's a registration task; the doc exists for traceability with the rest of the adoption set.
