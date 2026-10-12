# Landscape: Grafana dashboards in Git

Date: 2026-10-10 (sources fetched 2026-10-11). Short by design: the repo is a published JSON mirror, not a tool.

## What exists

| Project | What it does | Source |
|---|---|---|
| Grafana Git Sync | Stores dashboards as JSON in Git; bidirectional (UI saves to Git, repo changes load into Grafana); polls every 60 s, near-instant with webhooks; GitHub, GitLab, Bitbucket or plain Git. This repo is a Git Sync target. | https://grafana.com/docs/grafana/latest/as-code/observability-as-code/git-sync/ |
| Grafana Foundation SDK | Typed builders (Go, TypeScript, Python, PHP, Java) that emit dashboard JSON. Public preview, no SLA. | https://github.com/grafana/grafana-foundation-sdk |
| grafana/dashboard-linter | CLI that lints dashboards (`--strict`, `--fix`). README says early days; Prometheus datasources only. Apache-2.0. | https://github.com/grafana/dashboard-linter |
| Grizzly | CLI for observability resources as code. Archived 2026-06-05, to be replaced by grafanactl. Do not build on it. | https://github.com/grafana/grizzly |

## Better / worse than this repo

- Foundation SDK gives typed, reviewable source and consistent panels. This repo stores raw exported JSON, which is noisy in diffs.
- dashboard-linter catches missing units, bad datasource refs and unset variables. This repo has no checks at all (plan U2).
- Git Sync is bidirectional, but this repo is deliberately one-way: edits happen in the Pace-Server monorepo and are published here by script (README). A UI save in Grafana will diverge from the monorepo.

## What users expect

A LICENSE, CI that rejects broken JSON, and a list of what each dashboard needs (scrape targets, datasources). Plan U1 to U3 cover these.

## What is unique here

Eight Pace-authored dashboards on a shared `prometheus` / `loki` UID contract, plus ten community dashboards (grafana.com IDs listed in MANIFEST.md) rewritten to that contract. About 855 KB of the repo is community JSON; about 21 KB is Pace-authored.
