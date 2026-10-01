# river-data-repo: the founding plan and running record

**Rivers**, for the map. Created on GitHub by the owner and given its documents by the site's
`pipeline/scaffold/new-origin.py`. **Its rivers are made by the site's generator and pushed here by `enc-chart-repo`'s generate workflow with the land they fit, and a push under `map/` publishes them; its water gauges are fetched and published hourly, at twenty past.**

## What it is for

**The rivers where no chart reaches** (`map/nhd-water/`): the U.S. Geological Survey's National Hydrography Dataset water — streams and rivers, bays and inlets, canals and ditches and areas of complex channels (its large-scale Area layer) and estuaries (its large-scale Waterbody layer) — cut to the parts of US waters no NOAA nautical chart covers, within 0.05 degrees of charted water. Zoom-8 Mapbox vector tiles (layer `water`, extent 65536) under a version named by their own hash, and `nhd-water/index.json` naming the version, every tile and the version of `enc-chart-repo`'s `noaa-land/` they were cut to fit (`charts`). A reader counts these rings with that land's by the even-odd rule, so each is a hole in it: the York above West Point and a square of the Rappahannock were land before them. Pushed with that land by `enc-chart-repo`'s generate workflow and published on push. The world's rivers and their streamflow gauges are expected to follow.

**The water gauges** (`streamgauges.json`, since 2026-09-30): every U.S. Geological Survey monitoring location that reported streamflow (discharge, ft³/s) or water level (gage height, ft above the gauge's own datum) in the last ten days — about 11,000, most on streams, some on lakes, reservoirs, canals and estuaries — with its name, type, state, drainage area and point, fetched hourly by the site's `scripts/fetch-usgs-gauges.py`. The readings are USGS's own units and provisional, as USGS says of recent data. Each gauge's page is `https://waterdata.usgs.gov/monitoring-location/<id>/`.

## Where the data comes from

U.S. Geological Survey, the National Hydrography Dataset's map service (`https://hydro.nationalmap.gov/arcgis/rest/services/nhd/MapServer`, layers 9 and 12), a U.S. government work in the public domain. **Read 2026-09-30**: it answered a 0.05-degree box in 2 to 15 s with 1.5 to 2.7 MB of large-scale outline, timed out on a box a few times larger, and answered 502 and 504 often enough that every request is retried; the rivers needed 719 boxes. The cut is made by the same generator as `enc-chart-repo`'s land, kept in a private repository and run by that repository's generate workflow; nothing here runs it.

The gauges: USGS's Water Data OGC API (`https://api.waterdata.usgs.gov/ogcapi/v1`), its `latest-continuous` collection for the readings and `monitoring-locations` for names, a U.S. government work in the public domain. **Read 2026-09-30**: two pages of 10,000 series a parameter, 1.2 MB compressed, four seconds; names asked a hundred locations at a time, only for locations the last publish does not name and a twenty-fourth of the rest each hour. **It allows 1,000 requests an hour to an address without a key**, which answered 429 with a half-hour wait after a morning's exploration; `USGS_API_KEY` (a free key, api.waterdata.usgs.gov/signup) raises it.


## Open

1. A river layer with streamflow gauges, linking to each gauge's own page and data, expected.

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

**2026-09-30, evening.** Version `d71c8d904e` (23 zoom-8 tiles, 2,221 USGS
features), cut for `enc-chart-repo`'s `noaa-land/` `ea33a419d7`, pushed by
that repository's first generate run and published on the push
(`36781091333`, green). Besides the squares no chart covers, it now decides
the stretches only a small-scale chart covers where larger-scale charts
enclose them: the chart's water USGS maps as no water of any kind closes
(16.3 km², among it the Rappahannock's river drawn kilometres wide over its
south bank at 37.59 N) and USGS's rivers, bays, estuaries and sea within
0.05 degrees of charted water open (50.4 km², nearly all Delaware Bay's
marsh channels in New Jersey); North Carolina's sounds are unchanged. USGS's
sea, now in the query, opens two places no chart covers: open water north
of St Thomas that had been made land, and the St Croix River's upper reach.

**2026-09-30, night.** The water gauges declared: `streamgauges.json`,
about 11,000 USGS locations that reported streamflow or water level in ten
days, fetched hourly (`scripts/fetch-usgs-gauges.py` in the site). The first
dispatched run, `36793861915` (with `USGS_API_KEY` set), published 10,987
gauges, every one named — 8,721 with discharge, 10,938 with gage height —
to Pages and R2, green; the hourly schedule is on since, at twenty past.

