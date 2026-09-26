# AGENTS.md

Dashboards-only mirror for Grafana Git Sync on `observe.pacehq.io`.

## This is a mirror. Do not hand-edit.

**The source of truth is `duketopceo/Pace-Server` at
`docker/config/grafana/dashboards/`.** Changes are published by running a script
in that repository, not by committing here:

```bash
cd Pace-Server
bash scripts/operator/grafana-publish-dashboards-repo.sh
```

A hand edit made here is overwritten by the next publish. If you want a dashboard
changed, change it in Pace-Server and republish.

Git Sync reads branch `main`, path `.` (the repository root). Dashboards are
provisioned into the Grafana folder **Pace**.

## Provenance is part of the contract

`MANIFEST.md` splits these 18 files into two groups, and the distinction is
load-bearing:

- **8 Pace-authored originals** — `auth-health`, `agent-execution`,
  `container-health`, `error-rate`, `node-overview`, `onboarding-auth-billing`,
  `request-overview`, `sentry-overview`.
- **9 imported verbatim from grafana.com**, each with its dashboard ID recorded
  in `MANIFEST.md` (1860, 3662, 15798, 14282, 13639, 9628, 763, 11074, 15141) —
  `node-exporter-full`, `prometheus-overview`, `docker-monitoring`,
  `cadvisor-exporter`, `loki-stack`, `postgresql-database`, `redis-dashboard`,
  `traefik-2`, `loki-kubernetes-logs`.

**Do not reformat, re-indent, or "modernise" an imported dashboard.** These files
are third-party works carried with attribution. Editing them creates drift from
upstream and breaks the provenance record that `MANIFEST.md` exists to keep. If
an imported dashboard needs a change, fork it deliberately, move it into the
Pace-authored group in `MANIFEST.md`, and say why in the commit.

## Conventions

- One dashboard per file, named for its subject, not its Grafana title.
- Pace-authored dashboards use a `pace-` uid prefix and `Pace — ` title
  convention (for example `uid: pace-node-overview`, `title: Pace — Node Overview`).
  Preserve both when editing an existing one.
- Panels are Grafana JSON schema, not hand-written config. Change the JSON, do
  not convert it to another format.
- There is no build, no test, and no lint for this repository. Correctness is
  checked by Grafana refusing to provision the file, so a change here is
  validated in `observe.pacehq.io`, not in CI.
