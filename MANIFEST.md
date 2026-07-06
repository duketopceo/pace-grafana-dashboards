# Grafana dashboards (versioned in Git)

Provisioned from `docker/config/grafana/provisioning/dashboards/dashboards.yaml` into folder **Pace**.

## Pace-authored (repo originals)

| File | UID | Focus |
|------|-----|--------|
| `agent-execution.json` | (see file) | Agent runs |
| `auth-health.json` | pace-auth-health | Auth / Stytch |
| `container-health.json` | (see file) | Docker containers |
| `error-rate.json` | (see file) | Error rates |
| `node-overview.json` | (see file) | Node metrics |
| `onboarding-auth-billing.json` | (see file) | Onboarding funnel |
| `request-overview.json` | (see file) | HTTP requests |
| `sentry-overview.json` | (see file) | Sentry linkage |

## Imported from grafana.com

| File | grafana.com ID | Notes |
|------|----------------|-------|
| `node-exporter-full.json` | 1860 | Needs `node-exporter` scrape target |
| `prometheus-overview.json` | 3662 | Prometheus self-monitoring |
| `docker-monitoring.json` | 15798 | cAdvisor / Docker |
| `cadvisor-exporter.json` | 14282 | Container metrics |
| `loki-stack.json` | 13639 | Loki health |
| `postgresql-database.json` | 9628 | Postgres (pace-db) |
| `redis-dashboard.json` | 763 | Redis (pace-cache) |
| `traefik-2.json` | 11074 | pace-proxy / Traefik |
| `loki-kubernetes-logs.json` | 15141 | Log exploration |
| `alloy-monitoring.json` | 19419 | pace-alloy |

Datasources are normalized to `prometheus` and `loki` UIDs matching `provisioning/datasources/datasources.yaml`.

## Sync from live observe

```bash
bash scripts/operator/grafana-export-dashboards.sh   # on pace-prod-1
```

## Re-import community set

```bash
bash scripts/operator/grafana-import-community-dashboards.sh
```
