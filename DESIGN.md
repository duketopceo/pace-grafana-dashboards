# Design: dashboard conventions

Direction: **Master Clock**, Pace v5 (ratified 2026-10-04 in `duketopceo/Pace-Server` `DESIGN.md`; tokens in its `brand/tokens.json`, which win over this file). This repo has no app UI. This file only says how Pace dashboards use that direction. It invents nothing.

## Surfaces

Grafana dashboards on observe.pacehq.io and the README. Grafana owns the chrome and theme. Dashboard JSON only controls panels, units, thresholds and colours.

## Rules for Pace-authored dashboards

Applies to the eight `pace-*` dashboards. Community dashboards (see MANIFEST.md) are left as imported.

1. **Series colour**: graphite ramp `#16181A`, `#5E656B`, `#8A9096`, `#C4C8CA` (light) with signal `#D33C11` for "this tenant / now" only. Never more than 4 series per panel; split into more panels instead. Dark theme: signal `#FF6A3D`.
2. **Semantic colour only for thresholds**: success `#1E7A45`, warning `#F2B53A`, error `#A3123A` (dark: `#4FC27F`, `#E9B04A`, `#EF5C86`). Signal is never a status colour. Pair colour with a text value, since colour alone fails colour-blind readers.
3. **No blue**: info is graphite. Do not leave Grafana's default palette-classic on a panel.
4. **Units and numerals**: every numeric panel sets a unit. Stat and table values are tabular; readouts (times, IDs, rates) use the Pace mono face where the Grafana theme allows it.
5. **Titles**: sentence case, plain verbs, no em or en dashes in panel titles. (Existing dashboard titles still use "Pace — ..."; see roadmap U5.)
6. **Defaults**: `uid` prefixed `pace-`, tags `pace` plus area, datasource UIDs exactly `prometheus` and `loki`, default range `now-1h`.

## Status of the rules

Audit 2026-10-11: the eight Pace dashboards set no thresholds, colour overrides or units, so they follow Grafana defaults. Applying rules 1 to 4 is a content change that must happen in Pace-Server (`docker/config/grafana/dashboards/`), the source of truth, or the next publish overwrites it. No dashboard JSON is changed in this PR (roadmap U5).

## Contrast

Token contrast ratios (AA) are measured in Pace-Server `DESIGN.md` section 6.1; they are not re-measured here. Grafana panel backgrounds follow the Grafana theme, so verify series colours on both light and dark before adopting.

## Logo

Reused, not redrawn: interim Pace lockup copied from Pace-Server into `assets/brand/`. Owner signs off the final mark.
