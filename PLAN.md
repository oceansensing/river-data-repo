# river-data-repo: the founding plan and running record

**Rivers**, for the map. Created on GitHub by the owner and given its documents by the site's
`pipeline/scaffold/new-origin.py`. **Its rivers are made by the site's generator and pushed here by `enc-chart-repo`'s generate workflow with the land they fit, and a push under `map/` publishes them; its water gauges are fetched and published hourly, at twenty past.**

## What it is for

**The rivers where no chart reaches** (`map/nhd-water/`): the U.S. Geological Survey's National Hydrography Dataset water — streams and rivers, bays and inlets, canals and ditches and areas of complex channels (its large-scale Area layer) and estuaries (its large-scale Waterbody layer) — cut to the parts of US waters no NOAA nautical chart covers, within 0.05 degrees of charted water. Zoom-8 Mapbox vector tiles (layer `water`, extent 65536) under a version named by their own hash, and `nhd-water/index.json` naming the version, every tile and the version of `enc-chart-repo`'s `noaa-land/` they were cut to fit (`charts`). A reader counts these rings with that land's by the even-odd rule, so each is a hole in it: the York above West Point and a square of the Rappahannock were land before them. Pushed with that land by `enc-chart-repo`'s generate workflow and published on push. The world's rivers and their streamflow gauges are expected to follow.

**The water gauges** (`streamgauges.json`, since 2026-09-30): every U.S. Geological Survey monitoring location that reported streamflow (discharge, ft³/s) or water level (gage height, ft above the gauge's own datum) in the last ten days — about 11,000, most on streams, some on lakes, reservoirs, canals and estuaries — and, since that night, **every Water Survey of Canada station that did** (discharge, m³/s; water level, m; about 2,160, from Environment and Climate Change Canada's MSC GeoMet, OGL-Canada); since 2026-10-01 **every French hydrometric station that did** (about 3,170, metropolitan France and the overseas departments, from Hub'Eau's hydrometry API, Etalab's Licence Ouverte 2.0; water level in m and discharge in m³/s, converted from Hub'Eau's mm and L/s); since 2026-10-03 **every station that did of England's Environment Agency** (about 3,400, with the UK National Tide Gauge Network's coastal gauges once a site, each named by its nation), **Germany's federal waterways** (PEGELONLINE, about 680), **Ireland's Office of Public Works** (about 460, levels), **Switzerland's Federal Office for the Environment** (about 200), **Sweden's SMHI** (about 150, flows) and **Finland's Syke** (about 870, **daily values**, each site's newest day before today, marked `daily`), every reading in m and m³/s; and **the Arctic Great Rivers Observatory's thirteen Russian river stations**, whose measured days arrive eight to ten months late, so each carries its **typical flow for the day** — its mean on this day of the year over its record — marked `typical`, with its newest measured day (`measured`), its record's years (`record`) and its 366 daily means (`typicalByDay`). Each gauge has its name, place, drainage area and point, fetched hourly by the site's `scripts/fetch-stream-gauges.py`. Each reading names its unit and each gauge its agency (`USGS`, `WSC`, `FR`, `AGRO`, `EA`, `WSV`, `OPW`, `FOEN`, `SMHI` or `SYKE`), and a reading a day older than its station's newest is left out; the readings are provisional, and a typical flow is no reading, so it dates nothing in the header's `refTime`. A USGS gauge's page is `https://waterdata.usgs.gov/monitoring-location/<id>/`, a Canadian one's `https://wateroffice.ec.gc.ca/report/real_time_e.html?stn=<number>`, a French one's `https://www.hydro.eaufrance.fr/stationhydro/<code>/fiche`, an English one's `https://check-for-flooding.service.gov.uk/station/<ref>` where it has a `ref`, a German one's `https://www.pegelonline.wsv.de/gast/stammdaten?pegelnr=<number>`, an Irish one's `https://waterlevel.ie/00000<number>/0001/`, a Swiss one's `https://www.hydrodaten.admin.ch/de/seen-und-fluesse/stationen-und-daten/<number>`, and the Arctic rivers' `https://arcticgreatrivers.org/discharge/`.

## Where the data comes from

U.S. Geological Survey, the National Hydrography Dataset's map service (`https://hydro.nationalmap.gov/arcgis/rest/services/nhd/MapServer`, layers 9 and 12), a U.S. government work in the public domain. **Read 2026-09-30**: it answered a 0.05-degree box in 2 to 15 s with 1.5 to 2.7 MB of large-scale outline, timed out on a box a few times larger, and answered 502 and 504 often enough that every request is retried; the rivers needed 719 boxes. The cut is made by the same generator as `enc-chart-repo`'s land, kept in a private repository and run by that repository's generate workflow; nothing here runs it.

The gauges: USGS's Water Data OGC API (`https://api.waterdata.usgs.gov/ogcapi/v1`), its `latest-continuous` collection for the readings and `monitoring-locations` for names, a U.S. government work in the public domain. **Read 2026-09-30**: two pages of 10,000 series a parameter, 1.2 MB compressed, four seconds; names asked a hundred locations at a time, only for locations the last publish does not name and a twenty-fourth of the rest each hour. **It allows 1,000 requests an hour to an address without a key**, which answered 429 with a half-hour wait after a morning's exploration; `USGS_API_KEY` (a free key, api.waterdata.usgs.gov/signup) raises it.

France's: Hub'Eau's hydrometry API (`https://hubeau.eaufrance.fr/api/v2/hydrometrie`), eaufrance's, under Etalab's Licence Ouverte 2.0, no key. **Read 2026-10-01**: 4,178 stations in service and 9,306 sites, 4,506 with a drainage area, asked once a day and carried between; readings every five minutes at most stations, asked as ten minutes at whole hours — this hour, the two before and one six hours back, and on a first run one every twelve hours for ten days — which answered for 7,003 of an hour's 7,008 station-quantities; an hourly run is about 60,000 readings in two and a quarter minutes. Fifty-four stations carry their latitude and longitude swapped, and each is put back where its department's box says; one placed nowhere is left out.

The six of 2026-10-03, each one request for every station, open JSON, no key, **read 2026-10-03**: England's Environment Agency real-time flood-monitoring API (`https://environment.data.gov.uk/flood-monitoring`, the Open Government Licence v3.0 — *this uses Environment Agency flood and river level data from the real-time data API (Beta)*), its stations and every measure's newest reading, a station's stage over the stage below its weir, groundwater and rain left out, and drainage areas from its hydrology API once a day; Germany's PEGELONLINE (`https://www.pegelonline.wsv.de/webservices/rest-api/v2`, Datenlizenz Deutschland – Zero – 2.0), the federal waterways' stations with their current measurement, its neighbors' stations under their own agencies left out, a level in cm made m and 999.99 no value; Ireland's waterlevel.ie (`https://waterlevel.ie/geojson/latest/`, the Office of Public Works', re-use of public sector information), sensor 0001's level, and only stations 00001 to 41000, as OPW asks; Switzerland's hydrodaten (`https://www.hydrodaten.admin.ch/web-hydro-maps/hydro_sensor_pq.geojson`, the Federal Office for the Environment's, free to use with the source recommended, asked no more than once in ten minutes), whose map file states each station's unit (six publish L/s) and places it in Swiss LV95, a station the office says is out of order left out; Sweden's SMHI open hydrological observations (`https://opendata-download-hydroobs.smhi.se`, CC BY 4.0), the last hour's fifteen-minute discharge with each catchment's area; and Finland's Syke Hydrology API (`https://rajapinnat.ymparisto.fi/api/Hydrologiarajapinta/1.1/odata`, CC BY 4.0 — *Hydrologiarajapinta / Source: Finnish Environment Institute (Syke)*), daily values only, an estimate left out, a flow site and a level site within 300 m one gauge. A run of all ten agencies took under two minutes.

The Arctic rivers: the Arctic Great Rivers Observatory's discharge page (`https://arcticgreatrivers.org/discharge/`), Roshydromet's daily discharge, free to use for any purpose with credit to ArcticGRO. It has no feed: a 32 MB page carries each river's record as an embedded spreadsheet, re-versioned a few times a year. **Read 2026-10-01**: thirteen Russian stations still measuring, their newest days eight to ten months old. It is read on Mondays at 03 UTC, or when the last publish has none; between, the daily means kept in the last publish give each day's typical flow. Places from its discharge metadata, drainage areas from R-ArcticNET.


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

**2026-10-03.** Six more countries' gauges joined the file, each one
request for every station, open JSON, no key: England's Environment Agency
(its flood-monitoring API, the UK's coastal tide gauges once a site),
Germany's federal waterways (PEGELONLINE), Ireland's Office of Public Works
(waterlevel.ie), Switzerland's Federal Office for the Environment
(hydrodaten), Sweden's SMHI and Finland's Syke (daily values). Run from the
site's fetcher before its push, all ten agencies took 1 min 49 s on a first
run and 1 min 27 s on the next — the step has forty — and wrote 22,088
gauges, 5.8 MB (864 KB compressed), from 4.6 MB. The first publish with
them, run `37100477855` (begun 05:38 UTC), ran the step in about two
minutes and published 22,089 gauges — USGS 10,970, Canada 2,161, France
3,145, England 3,434, Germany 681, Ireland 459, Switzerland 201, Sweden
155, Finland 870, Arctic GRO 13 — every server `ok` in the status.

**2026-10-01.** France's and the Arctic Great Rivers Observatory's gauges
joined the file: the site's fetcher asks Hub'Eau's hydrometry API at whole
hours (ten minutes a window; a first run one every twelve hours for ten
days) and reads Arctic GRO's discharge page on Mondays, the Russian rivers
published by their typical flow for the day. A first run with nothing
carried timed at 18 minutes for France alone, against the step's twenty,
so the gauges' step has forty (`products.toml`). The first publish with
them, run `36869687921`, built in 14½ minutes and published 16,291 gauges
to Pages and R2 at 13:49 UTC — USGS 10,967, Water Survey of Canada 2,162,
France 3,149, Arctic GRO 13 — the header's `refTime` 13:31Z, a typical flow
dating nothing.

**2026-09-30, later that night.** Canada's water gauges joined the same
file: the site's fetcher, now `scripts/fetch-stream-gauges.py`, asks MSC
GeoMet for the active stations and for the readings at whole hours (the
hour and the three before it; a first run one every twelve hours for ten
days), carries a station's newest reading for ten days, and keeps one
agency's gauges from the last publish while the other fails. First live run
on the site's machine: 13,150 gauges, 2,161 of them Canada's, 25 s.

**2026-09-30, night.** The water gauges declared: `streamgauges.json`,
about 11,000 USGS locations that reported streamflow or water level in ten
days, fetched hourly (`scripts/fetch-usgs-gauges.py` in the site, now
`fetch-stream-gauges.py`). The first
dispatched run, `36793861915` (with `USGS_API_KEY` set), published 10,987
gauges, every one named — 8,721 with discharge, 10,938 with gage height —
to Pages and R2, green; the hourly schedule is on since, at twenty past.

