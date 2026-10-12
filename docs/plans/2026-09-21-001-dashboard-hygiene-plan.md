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
**Status:** not started (no LICENSE on main as of 2026-10-11).
**Difficulty:** easy. One file.
**Feasibility:** needs owner confirmation of MIT; community dashboards keep their grafana.com licences, so say so in the README.
**Simpler alternative:** none; it is already one file.
**Verification:** `LICENSE` present; GitHub detects MIT.

### U2 — JSON validation CI
**Files:** `.github/workflows/validate.yml` (new)
- On push + PR: parse every `*.json`; fail on invalid JSON. Optional later: check each
  dashboard has `uid` and `title`.
**Status:** not started (no `.github/` on main).
**Difficulty:** easy. `jq empty` over `*.json` plus a `uid`/`title` check.
**Feasibility:** public repo, hosted Actions run normally. Syntactic only; no Grafana in CI.
**Simpler alternative:** a 10-line shell script run by the workflow; do not adopt grafana/dashboard-linter yet (early days, Prometheus only, https://github.com/grafana/dashboard-linter). Also provides the local check: `for f in *.json; do jq empty "$f" || exit 1; done`.
**Verification:** a PR with broken JSON fails the workflow.

### U3 — Complete prerequisites in MANIFEST.md
**Files:** `MANIFEST.md`
- Every dashboard row gets what it needs: scrape targets (`node-exporter`, …),
  datasources (Prometheus, Loki), and folder. Some rows already note this — finish the set.
**Status:** partial (some rows name a scrape target).
**Difficulty:** medium. Needs reading each JSON's `datasource` and `job` selectors; the two large imports (`node-exporter-full`, `traefik-2`) take time.
**Feasibility:** can be generated with jq from the JSON itself, so it cannot drift from the files.
**Simpler alternative:** generate the table with a small jq script (also checks `uid`) instead of hand-editing; the `(see file)` UID cells go away.
**Verification:** a new on-call can provision from the table alone.

### U4 — Pace dashboards follow DESIGN.md (Next)
**Files:** upstream in Pace-Server `docker/config/grafana/dashboards/pace-*.json`, then republished here.
- Add units, thresholds and colours per `DESIGN.md`. Audit: the eight Pace dashboards set none.
**Status:** not started. **Difficulty:** medium (about 25 panels, but visual review needed). **Feasibility:** blocked on changing the monorepo, not this repo. **Simpler alternative:** apply one shared fieldConfig default via a jq transform in the publish script rather than hand-editing each panel.
**Verification:** `jq empty` passes; panels checked in light and dark.

### U5 — Drift check (Next)
**Files:** `.github/workflows/validate.yml`
- Git Sync is bidirectional, so a Grafana UI save can diverge from the monorepo (https://grafana.com/docs/grafana/latest/as-code/observability-as-code/git-sync/). Fail or warn when this repo differs from the monorepo source.
**Status:** not started. **Difficulty:** medium. **Feasibility:** needs read access to the private Pace-Server repo from CI. **Simpler alternative:** make the Grafana Git Sync instance read-only for this folder and skip the check.
**Verification:** a manual UI edit is flagged.

### U6 — Community dashboards (Later)
**Files:** the ten imports in MANIFEST.md (about 855 KB of 876 KB; `node-exporter-full.json` alone is 467 KB).
**Status:** open decision. **Difficulty:** medium. **Feasibility:** provisioning by grafana.com ID needs outbound network and loses the `prometheus`/`loki` UID rewrite. **Simpler alternative (biggest in this repo):** vendor only the eight Pace dashboards and keep the community set in the monorepo import script, which already exists (`grafana-import-community-dashboards.sh`).
**Verification:** Grafana provisions the same set after the change.

## Key decisions

- Validation is syntactic only — no Grafana instance in CI; semantic checks stay manual.
- This repo stays a **dumb sync target**: all dashboard edits happen upstream.

## Assumptions / open questions

- MIT license assumed consistent with owner's other repos; confirm if Pace needs otherwise.
