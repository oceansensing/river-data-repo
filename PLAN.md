# river-data-repo: the founding plan and running record

**Rivers**, for the map. Created on GitHub by the owner and given its documents by the site's
`pipeline/scaffold/new-origin.py`. **Its files are made by the site's generator and pushed here by `enc-chart-repo`'s generate workflow with the land they fit; a push under `map/` publishes them.**

## What it is for

**The rivers where no chart reaches** (`map/nhd-water/`): the U.S. Geological Survey's National Hydrography Dataset water — streams and rivers, bays and inlets, canals and ditches and areas of complex channels (its large-scale Area layer) and estuaries (its large-scale Waterbody layer) — cut to the parts of US waters no NOAA nautical chart covers, within 0.05 degrees of charted water. Zoom-8 Mapbox vector tiles (layer `water`, extent 65536) under a version named by their own hash, and `nhd-water/index.json` naming the version, every tile and the version of `enc-chart-repo`'s `noaa-land/` they were cut to fit (`charts`). A reader counts these rings with that land's by the even-odd rule, so each is a hole in it: the York above West Point and a square of the Rappahannock were land before them. Pushed with that land by `enc-chart-repo`'s generate workflow and published on push. The world's rivers and their streamflow gauges are expected to follow.

## Where the data comes from

U.S. Geological Survey, the National Hydrography Dataset's map service (`https://hydro.nationalmap.gov/arcgis/rest/services/nhd/MapServer`, layers 9 and 12), a U.S. government work in the public domain. **Read 2026-09-30**: it answered a 0.05-degree box in 2 to 15 s with 1.5 to 2.7 MB of large-scale outline, timed out on a box a few times larger, and answered 502 and 504 often enough that every request is retried; the rivers needed 719 boxes. The cut is made by the same generator as `enc-chart-repo`'s land, kept in a private repository and run by that repository's generate workflow; nothing here runs it.

## Open

1. The first dispatched run, read: Pages and R2.
2. A river layer with streamflow gauges, linking to each gauge's own page and data, expected.

## Record

**2026-09-30.** First publish, run `36751215526`: `nhd-water/` version
`095574cab8` (19 zoom-8 tiles, 1,252 USGS features), cut for `enc-chart-repo`'s
`noaa-land/` `012b39a3e7`; build, Pages and R2 green; listed in the site's
`MAP_ORIGINS` after it. It opened the York above West Point (37°30′ N) and a
square of the Rappahannock no chart covers.

**Decided 2026-09-30, to build next**: `enc-chart-repo`'s monthly workflow
regenerates these rivers with its land and pushes them here, so the two
publish together; a run that changes nothing leaves the published set as
it is. **Held**: a second set (87 tiles) from a rule letting USGS decide
squares only a small-scale chart covers, which closed the Great Lakes'
open water (USGS maps them as lakes); not published.
