# Data sources

Every dataset MyPreflight uses or plans to use, with licence and refresh method.

Last reviewed: 2026-11-16

## Rules

- Open or properly licensed data only, always attributed where required.
- Free sources first. A self-funded project spends money, even low hundreds, only with very good reasoning.
- Never judge pilots on data we can't trust.
- Generated content follows deterministic rules from real context (origin city, airline, aircraft).

## External datasets

| Data | Used for | Source | Licence / attribution | Refresh | Status |
| --- | --- | --- | --- | --- | --- |
| Flight plans | Import, OFP, weights, times, runway, crew roster, waypoints, ETOPS points | SimBrief API (the pilot's own account) | SimBrief terms | Per import | Live |
| Airports, runways, terminals, stands, gates, boundaries | Briefing, maps, automatic takeoff and landing detection | OpenStreetMap | ODbL, attribution required | On demand: operations pull a proposal per airport and apply the changes they accept. | Live |
| Basic airport records | Creating an airport a SimBrief import mentions but we don't hold yet; operations look-up by IATA code | SkyLink API | **Open:** confirm terms | Once, when the airport is created; OpenStreetMap enrichment follows | Live |
| Seat maps | Manifests, seating, cabin capacity | AeroLOPA | **Open:** confirm licence. **Open:** get access to seat positions, which AeroLOPA no longer publishes openly (noticed in November 2026) | Catalogue mirrored when operations sync it; seat data fetched on first use and stored as revisions, refreshed on request. Layouts already stored still show; a layout not stored yet can't be fetched, and a refresh is refused, until we have that access | Partial |
| METAR, TAF | Weather briefing | aviationweather.gov | **Open:** confirm terms | While any flight using the airport is active (checked in through taxiing in) | Live (raw text) |
| ATIS, METAR, TAF | Weather briefing, second source | SayIntentions | **Open:** confirm terms | Same as above | Live (raw text) |
| Real-world ATIS, US | Weather briefing: the digital ATIS of about 76 US airports, arrival and departure apart where an airport publishes both | The FAA's digital ATIS, relayed by the community site atis.info | **Open:** confirm terms; the relay publishes none, and the FAA marks its data not for operational use | Same as above. An ATIS the airport stops publishing is removed | Live |
| Real-world ATIS, worldwide where requested | Weather briefing: real digital ATIS for airports outside the US, arrival and departure apart | atis.guru, which collects the digital ATIS that aircraft request over datalink | **Open:** confirm terms; the site publishes none and runs on ads and donations, so ask its owner before pilots rely on it | Same as above. Only as fresh as the last aircraft that asked for it, so an ATIS no aircraft requested in the last 6 hours is dropped | Live |
| VATSIM ATIS | Weather briefing: the ATIS of VATSIM controllers online at the airport | VATSIM's public data feed | **Open:** confirm terms | Same as above. Removed when the controller logs off | Partial (collected, not shown) |
| Runways in use | Runway card and runway picker: the runways each ATIS puts in use for arrivals and for departures | Read from the ATIS text by fixed rules. An ATIS the rules can't settle goes to OpenAI's small model, which may pick only from the airport's runways; see [decisions](decisions.md) | OpenAI terms. Only the ATIS text and the airport's runway designators are sent | Once per ATIS, when its text changes. If OpenAI can't be reached, the rules' own reading stands and the ATIS is asked about again later | Live |
| NOTAMs | Briefing | SimBrief OFP | SimBrief terms | Import time only | Live (raw) |
| Pacific oceanic tracks (PACOTS) | The Pacific tracks published now, westbound from Oakland and eastbound from Fukuoka | FAA NOTAM Management Service, with a key the FAA issued to MyPreflight | **Open:** confirm terms. **Open:** the key reaches the FAA's pre-production service only; ask the FAA for production access before pilots rely on the tracks | Read again at most every 15 minutes; the FAA allows one read every 3 minutes | Partial |
| North Atlantic oceanic tracks (NAT) | The North Atlantic tracks published now, eastbound from Gander and westbound from Shanwick, with the flight levels of each and the message's remarks, the TMI among them | The FAA's public North Atlantic track page, which needs no key | **Open:** confirm terms; the page publishes none | Read with the Pacific tracks, at most every 15 minutes. When it can't be read, the Pacific tracks still show | Partial |
| Positions of oceanic track fixes | Placing each fix of a Pacific or North Atlantic track on the map | FAA aeronautical data: designated points and navaids, current for US airspace, and an archive last edited in 2016 for the rest, Japan and most North Atlantic entry and exit points included. A fix none of them holds is shown as unplaced, never guessed; some North Atlantic entry and exit points are newer than the archive, so those tracks end at their last coordinate | US public data | Each look-up kept for a day | Partial |
| FIR boundaries and names | Airspace layer on the flight maps, and FIR names in the fix card | VATSpy Data Project, VATSIM's controller map, corrected by hand where VATSIM splits or names a FIR differently from the real one, or leaves out an upper area a flight plan can name | CC BY-SA 4.0: attribution, and our adapted copy carries the same licence | Checked weekly; a refresh is proposed when VATSpy changes, usually each AIRAC cycle | Live |
| Map tiles | Every map, flat and globe | VersaTiles Positrino and Positrino Dark, built from OpenStreetMap; CARTO raster basemaps as the fallback | Attribution required (OpenStreetMap, VersaTiles, ESA WorldCover; CARTO when the fallback shows) | Live tiles from VersaTiles' free public service | Live |
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
| Cargo hold layouts | Loading cargo into holds and ULD positions | Curated per airframe type | With product releases | Live |
| Airframe types | Aircraft records, freighter vs passenger | Curated | With product releases | Live |
| Countries | Country of every airport, passport stamps | ISO 3166-1 alpha-2 list, curated | With product releases | Live |
| Crew | A plausible roster around the pilot (first officer, purser, cabin crew) | Imported from the SimBrief roster; they never sign in | Per import | Live |
| Postcards | Collectibles | AI-generated artwork, one per city, with the OpenAI image API | Once per new city; operations can redraw | Partial |

## Searched, nothing usable yet

- Free NOTAM API covering Europe (the FAA NOTAM API is US-centric).
- Open noise abatement and curfew database.
- Free procedure database outside the US (Navigraph is unlikely to share data for free).
- Official ICAO FIR boundaries: ICAO sells them (from about USD 3,000), not under an open licence.
