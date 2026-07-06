# pace-grafana-dashboards

Dashboards-only repo for [Grafana Git Sync](https://grafana.com/docs/grafana/latest/as-code/observability-as-code/git-sync/) on observe.pacehq.io.

Source of truth in the monorepo: `duketopceo/Pace-Server` → `docker/config/grafana/dashboards/`.

Publish updates:

```bash
cd Pace-Server
bash scripts/operator/grafana-publish-dashboards-repo.sh
```

Git Sync settings: branch `main`, path `.` (repo root).
