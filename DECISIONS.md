# Decisions

Dated, irreversible-leaning decisions, one entry each, newest last. The
reasoning lives in `PLAN.md`.

**What counts as one-way in a data repository**: a decision that puts bytes in
readers' hands under a shape they will code against; a decision about which
repository owns a product, since moving one costs a migration in two places;
and a decision that forecloses an upstream.

## D1 — 2026-09-30 — Its own repository, for the world's rivers

Rivers get a repository of their own (the owner, 2026-09-30), to hold the world's rivers and their streamflow gauges in time, not only the United States'. Its first product is USGS's rivers where NOAA's charts stop, cut to fit `enc-chart-repo`'s land. **Published on dispatch, never on a schedule**, as that land is.

## D2 — 2026-09-30 — Pushed here by enc-chart-repo's generate workflow, published on push

The rivers are made with the land they fit, by the site's generator run in `enc-chart-repo`'s generate workflow, which pushes them here with a deploy key that can write to this repository (its private half is that repository's `RIVERS_DEPLOY_KEY`); a push under `map/` starts this repository's publish. D1's "never on a schedule" holds for this workflow — it runs when files arrive — and the files arrive monthly at most. One-way in that a credential outside this repository can write to it; revoking the deploy key ends it.

## D3 — 2026-09-30 — A live product: the water gauges, hourly

Beside the monthly rivers, this repository publishes USGS's water gauges — every location that reported streamflow or water level in the last ten days, any site type — fetched hourly from USGS's Water Data API by the site's fetcher. Its first scheduled publishing: once a dispatched run has published, the workflow's cron runs at twenty past each hour, and the status watches the product against a three-hour budget. One-way in the ordinary sense: readers code against `streamgauges.json`'s shape, which the site's contract holds (`StreamGaugeFile`). Rivers and their gauges belong together here, which is why the gauges are not in `realtime-data-repo`.

