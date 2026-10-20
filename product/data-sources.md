# Data sources

Every dataset MyPreflight uses or plans to use, with licence and refresh method.

Last reviewed: 2026-10-20

## Rules

- Open or properly licensed data only, always attributed where required.
- Free sources first. A self-funded project spends money, even low hundreds, only with very good reasoning.
- Never judge pilots on data we can't trust.
- Generated content follows deterministic rules from real context (origin city, airline, aircraft).

## External datasets

| Data | Used for | Source | Licence / attribution | Refresh | Status |
| --- | --- | --- | --- | --- | --- |
| Flight plans | Import, OFP, weights, times, runway, crew roster, waypoints, ETOPS points | SimBrief API (the pilot's own account) | SimBrief terms | Per import | Live |
| Airports, runways, terminals, stands, gates, boundaries | Briefing, maps, automatic takeoff and landing detection | OpenStreetMap through the OSM provider function | ODbL, attribution required | On demand: operations pull a proposal per airport and apply the changes they accept. Pulls are cached for an hour. | Live |
| Basic airport records | Creating an airport a SimBrief import mentions but we don't hold yet; operations look-up by IATA code | SkyLink API | **Open:** confirm terms | Once, when the airport is created; OpenStreetMap enrichment follows | Live |
| Seat maps | Manifests, seating, cabin capacity | AeroLOPA through the AeroLOPA provider function | **Open:** confirm licence | Catalogue mirrored when operations sync it; seat data fetched on first use and stored as revisions, refreshed on request | Live |
| METAR, TAF | Weather briefing | aviationweather.gov | **Open:** confirm terms | While any flight using the airport is active (checked in through taxiing in) | Live (raw text) |
| ATIS, METAR, TAF | Weather briefing, second source | SayIntentions | **Open:** confirm terms | Same as above | Live (raw text) |
| NOTAMs | Briefing | SimBrief OFP | SimBrief terms | Import time only | Live (raw) |
| Map tiles | Every map | CARTO raster basemaps | Attribution required (CARTO and OpenStreetMap) | Live tiles | Live |
| Passenger counts | Boarding, deboarding | GSX Pro API through the Transponder | GSX terms | Live | Partial |
| US procedures (SIDs, STARs, approaches) | Route drawing | FAA CIFP | Free, US public data | Per AIRAC cycle | Planned, raised priority |
| Aircraft registrations | Auto-creating unknown airframes | OpenSky Network aircraft database | **Open:** check licence | {to decide} | Research |
| Noise abatement, curfews | Briefing | National AIPs through the AIP analyzer | Per state | {to decide} | Planned after launch |

`{to decide}` marks a refresh method nobody has chosen yet, because the source is still in research or planning.

## Curated and generated data

| Data | Used for | How it's made | Refresh | Status |
| --- | --- | --- | --- | --- |
| Passenger manifests | Briefing | Generated, deterministic, fake names, IATA special service request codes | Per loadsheet | Live |
| Cargo | Briefing | Generated, deterministic, origin-aware (tulips from Amsterdam, salmon from Keflavik), from a curated catalogue of 100 commodities with origins and seasons | Per loadsheet | Live |
| Cargo hold layouts | Loading cargo into holds and ULD positions | Curated per airframe type, in the backend repo | With code releases | Live |
| Airframe types | Aircraft records, freighter vs passenger | Curated, in the backend repo | With code releases | Live |
| Countries | Country of every airport, passport stamps | ISO 3166-1 alpha-2 list, curated | With code releases | Live |
| Crew | A plausible roster around the pilot (first officer, purser, cabin crew) | Imported from the SimBrief roster; they never sign in | Per import | Live |
| Postcards | Collectibles | AI-generated artwork, one per city, through the postcard generator (OpenAI image API) | Once per new city; operations can redraw | Partial |

## Searched, nothing usable yet

- Free NOTAM API covering Europe (the FAA NOTAM API is US-centric).
- Open noise abatement and curfew database.
- Free procedure database outside the US (Navigraph is unlikely to share data for free).
