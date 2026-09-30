# river-data-repo

**Rivers**, for the map: a data repository of the oceansensing ocean map system, with its own
Pages site, its own schedule and its own gigabyte, and no code of its own.

**Its rivers are made by the site's generator and pushed here by `enc-chart-repo`'s generate workflow with the land they fit, and a push under `map/` publishes them; its water gauges are fetched hourly.** `PLAN.md` is the founding plan; `CLAUDE.md` carries what must not be
got wrong and the shared doc doctrine.

## What it publishes

**The rivers where no chart reaches** (`map/nhd-water/`): the U.S. Geological Survey's National Hydrography Dataset water — streams and rivers, bays and inlets, canals and ditches and areas of complex channels (its large-scale Area layer) and estuaries (its large-scale Waterbody layer) — cut to the parts of US waters no NOAA nautical chart covers, within 0.05 degrees of charted water. Zoom-8 Mapbox vector tiles (layer `water`, extent 65536) under a version named by their own hash, and `nhd-water/index.json` naming the version, every tile and the version of `enc-chart-repo`'s `noaa-land/` they were cut to fit (`charts`). A reader counts these rings with that land's by the even-odd rule, so each is a hole in it: the York above West Point and a square of the Rappahannock were land before them. Pushed with that land by `enc-chart-repo`'s generate workflow and published on push. The world's rivers and their streamflow gauges are expected to follow.

These products are published **operationally but not drawn on the website's map**; the map's status
line still reports them when they fall behind, which is how their health stays visible.

**The water gauges** (`streamgauges.json`, since 2026-09-30): every U.S. Geological Survey monitoring location that reported streamflow (discharge, ft³/s) or water level (gage height, ft above the gauge's own datum) in the last ten days — about 11,000, most on streams, some on lakes, reservoirs, canals and estuaries — with its name, type, state, drainage area and point, fetched hourly by the site's `scripts/fetch-usgs-gauges.py`. The readings are USGS's own units and provisional, as USGS says of recent data. Each gauge's page is `https://waterdata.usgs.gov/monitoring-location/<id>/`.

## Where the data comes from

U.S. Geological Survey, the National Hydrography Dataset's map service (`https://hydro.nationalmap.gov/arcgis/rest/services/nhd/MapServer`, layers 9 and 12), a U.S. government work in the public domain. **Read 2026-09-30**: it answered a 0.05-degree box in 2 to 15 s with 1.5 to 2.7 MB of large-scale outline, timed out on a box a few times larger, and answered 502 and 504 often enough that every request is retried; the rivers needed 719 boxes. The cut is made by the same generator as `enc-chart-repo`'s land, kept in a private repository and run by that repository's generate workflow; nothing here runs it.

The gauges: USGS's Water Data OGC API (`https://api.waterdata.usgs.gov/ogcapi/v1`), its `latest-continuous` collection for the readings and `monitoring-locations` for names, a U.S. government work in the public domain. **Read 2026-09-30**: two pages of 10,000 series a parameter, 1.2 MB compressed, four seconds; names asked a hundred locations at a time, only for locations the last publish does not name and a twenty-fourth of the rest each hour. **It allows 1,000 requests an hour to an address without a key**, which answered 429 with a half-hour wait after a morning's exploration; `USGS_API_KEY` (a free key, api.waterdata.usgs.gov/signup) raises it.


## How it runs

The orchestrator (the site's private `pipeline/`), the fetchers and the
published-file contract all come from `oceansensing.github.io`, checked out at
run time. This repository carries `pipeline/products.toml` and its publish
workflow, and nothing else executable; a push under `map/` starts the publish,
and `enc-chart-repo`'s generate workflow pushes here with a deploy key that can write. Each run publishes to GitHub Pages and
to Cloudflare R2 from one build. Related repositories: `enc-chart-repo`, whose land these rivers are cut to fit.

**Which document gets what, and what "update docs" means across all
twenty-four repositories, is the doctrine block at the top of `CLAUDE.md`**: the
same text in all twenty-four, held equal by the site's `check:docs`.

## Structure

```
README.md       what this is
CLAUDE.md       what must not be got wrong, and the shared doc doctrine
PLAN.md         the founding plan and running record
DECISIONS.md    dated one-way decisions, D1 onward
pipeline/       products.toml, the declaration the orchestrator reads
map/            the committed files, published as they are
.github/        the publish workflow
```
