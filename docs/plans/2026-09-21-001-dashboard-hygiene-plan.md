# Plan: pace-grafana-dashboards → 10/10

**Date:** 2026-09-21 · **Status:** proposed · **Depth:** lightweight
**Origin:** repo scorecard pass — engineering rigor 7, docs 5, OSS citizenship 6.

## Problem frame

A dashboards-only repo synced from the Pace monorepo via Grafana Git Sync. `MANIFEST.md`
already catalogs the dashboards well, but the repo has **no license, no validation, and
no prerequisites list** — a malformed JSON or a missing scrape target silently ships a
broken dashboard to production observability.

## Scope

**In:** license, JSON validation CI, completed prerequisites in MANIFEST.
**Out:** dashboard content changes (those happen in the monorepo source of truth).

## Implementation units

### U1 — License
**Files:** `LICENSE` (new)
- MIT, matching the owner's other repos. One-line README note.
**Test scenarios:** n/a.

### U2 — JSON validation CI
**Files:** `.github/workflows/validate.yml` (new)
- On push + PR: parse every `*.json`; fail on invalid JSON. Optional later: check each
  dashboard has `uid` and `title`.
**Test scenarios:** open a PR with a deliberately broken JSON → workflow fails.

### U3 — Complete prerequisites in MANIFEST.md
**Files:** `MANIFEST.md`
- Every dashboard row gets what it needs: scrape targets (`node-exporter`, …),
  datasources (Prometheus, Loki), and folder. Some rows already note this — finish the set.
**Test scenarios:** n/a — review criterion: a new on-call can provision from the table alone.

## Key decisions

- Validation is syntactic only — no Grafana instance in CI; semantic checks stay manual.
- This repo stays a **dumb sync target**: all dashboard edits happen upstream.

## Assumptions / open questions

- MIT license assumed consistent with owner's other repos; confirm if Pace needs otherwise.
