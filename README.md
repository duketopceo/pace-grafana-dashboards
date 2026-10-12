<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/brand/pace-lockup-reversed.svg">
    <img src="assets/brand/pace-lockup.svg" alt="Pace" height="40">
  </picture>
</p>

# pace-grafana-dashboards

Grafana dashboards for Pace, published as JSON for [Grafana Git Sync](https://grafana.com/docs/grafana/latest/as-code/observability-as-code/git-sync/) on observe.pacehq.io.

This repo is a **publish target**, not the source. Dashboards are edited in `duketopceo/Pace-Server` under `docker/config/grafana/dashboards/` and copied here. Edits made only here or in the Grafana UI can be overwritten by the next publish.

## Contents

18 dashboards at the repo root: 8 Pace-authored, 10 imported from grafana.com. See [MANIFEST.md](MANIFEST.md) for the list, UIDs and prerequisites. Datasources use the UIDs `prometheus` and `loki`.

## Use

Publish (from the Pace-Server checkout):

```bash
bash scripts/operator/grafana-publish-dashboards-repo.sh
```

Git Sync settings: branch `main`, path `.` (repo root).

Check the JSON locally:

```bash
for f in *.json; do jq empty "$f" || echo "bad: $f"; done
```

## Docs

- [DESIGN.md](DESIGN.md): panel, colour and threshold conventions (Pace "Master Clock")
- [ROADMAP.md](ROADMAP.md): planned work
- [docs/research/2026-10-10-landscape.md](docs/research/2026-10-10-landscape.md): similar tools

## Status

No licence file yet (roadmap U1). No CI yet (U2). The logo is the interim Pace lockup from Pace-Server; the owner signs off the final mark.
