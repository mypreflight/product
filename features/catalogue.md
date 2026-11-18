# Feature catalogue

Every MyPreflight feature and its build status, taken from what's actually built, not from the brief.

Last reviewed: 2026-11-18

## How to read this

| Status | Meaning |
| --- | --- |
| Live | Built end to end and reachable in the app, the Transponder or the website |
| Partial | Some of it exists, but pilots can't use all of it yet |
| Planned | Queued for development, not built yet |

"Decision" points to the matching section of `../product/decisions.md`.

Before advertising anything, check its status here. Only **Live** features go in marketing: advertising something unbuilt breaks the "Trust first" hard line in `../product/overview.md`.

## Briefing

| Feature | In user language | Status | Decision |
| --- | --- | --- | --- |
| SimBrief import | Import your latest SimBrief plan yourself and fly it straight away, checked in with its estimated times | Live | SimBrief import |
| Briefing by phase | The briefing in three tabs that follow the flight, Preflight, Oceanic (En route without a North Atlantic track) and Arrival, opening on the one you're in. The nav log, OFP, runway analysis, NOTOC and PIL open in a side panel | Live | — |
| OFP viewer | Read the full SimBrief OFP in the app | Live | SimBrief import |
| Runway analysis | SimBrief's runway analysis page, one click from the briefing | Live | SimBrief import |
| Loadsheets | Preliminary revisions and a final loadsheet, never overwritten | Live | Passengers and cargo |
| Fuel ladder | Fuel build-up from taxi to total, with tank gauge | Live | SimBrief import |
| Passenger manifest | Named passengers seated in the real cabin, with no-shows | Live | Passengers and cargo |
| Special assistance | Infants, wheelchairs, unaccompanied minors, by real IATA codes; the PIL lists them, filtered by code | Live | Passengers and cargo |
| Seat maps | The airline's real seat map, seat by seat | Partial | Passengers and cargo |
| Cabin layout assignment | Operations pick the layout each aircraft flies | Live | Passengers and cargo |
| Cargo manifest | Shipments with waybills, origins and onward connections | Live | Passengers and cargo |
| Origin-aware cargo | Tulips from Amsterdam, salmon from Keflavik, in season | Live | Passengers and cargo |
| Cargo in the hold | Containers and pallets drawn in the aircraft's real hold | Live | Passengers and cargo |
| Cold chain | Will the salmon stay cold? Risk with a plain-language reason | Live | Passengers and cargo |
| NOTOC | Notification to captain with dangerous goods and drills, printable | Live | Passengers and cargo |
| Baggage | Bags derived from the payload, loaded with the cargo | Live | Passengers and cargo |
| Crew roster | First officer, purser and cabin crew from SimBrief | Live | SimBrief import |
| Weather | Raw ATIS, METAR and TAF from two sources, your choice | Live | Weather and NOTAMs |
| Real-world ATIS | Real digital ATIS for US airports and many others, arrival and departure apart, next to SayIntentions' ATIS. Pick either as your default, and switch in the briefing | Live | Weather and NOTAMs |
| Runways in use and runway wind | The runway card and the runway picker mark the runways the ATIS has in use for your departure or arrival, with its information letter, and give the wind along and across each runway | Live | Weather and NOTAMs |
| VATSIM ATIS | The ATIS of VATSIM controllers online at the airport | Partial (collected, not shown) | Weather and NOTAMs |
| NOTAMs | The NOTAMs from your OFP, per airport | Live | Weather and NOTAMs |
| Gate, stand and runway | One card per airport in the briefing, its weather beside the stand and the runway, with the stand's position for aligning the IRS. Change gate, stand and departure or arrival runway, like an EFB, picking stands and runways on the airport map | Live | Airports |
| Planned route | The route drawn fix by fix | Partial | Tracking, maps and flight pages |
| ETOPS briefing | Entry, exit and equal-time points with diversion airports | Partial | — |
| Oceanic tracks | The North Atlantic tracks your flight was planned against | Partial | — |
| Discord briefing | Your briefing and loadsheets as Discord DMs | Live | Discord bot |

## Flying and tracking

| Feature | In user language | Status | Decision |
| --- | --- | --- | --- |
| Flight progress | Check in, board, off-block, takeoff, landing, on-block, close | Live | Flight lifecycle |
| Automatic off-block | Detected from ground speed above 3 kt | Live | Flight lifecycle |
| Automatic takeoff | Detected when you leave the departure airport's boundary | Live | Flight lifecycle |
| Automatic landing | Detected inside the destination boundary below 50 kt | Live | Flight lifecycle |
| Actual fuel burn | Type the fuel you burned when you close the flight | Live | Flight lifecycle |
| Actual fuel at fixes | While in cruise, report the fuel on board at any fix from the fix card, the nav log or the briefing's Oceanic or En route tab, with the time it was read, and read it against the plan, with a warning below the minimum. The fuel card lists every report | Live | Flight lifecycle |
| Transponder | One portable .exe that feeds your position from MSFS 2024 | Live | Transponder |
| Auto standby | The Transponder switches on at boarding and off at on-block | Live | Transponder |
| Transponder updates | One key saves the new version to Downloads, checksum verified | Live | Transponder |
| Live map tracking | Your aircraft moving on the map with live telemetry | Live | Tracking, maps and flight pages |
| Vertical profile | Your altitude and ground speed over time against the planned altitude, under the Tracking view's map as you fly and on every finished flight. Point at a moment to put the aircraft there on the map, and zoom in on any part of the flight, down to five minutes | Live | Tracking, maps and flight pages |
| Globe maps | Every route map on a round earth or flat, your choice per map | Live | Tracking, maps and flight pages |
| FIR boundaries | The flight information regions you fly through, named along each border as on an enroute chart | Live | Tracking, maps and flight pages |
| North Atlantic tracks | The North Atlantic tracks in force now, eastbound from Gander and westbound from Shanwick, on the same maps as the Pacific tracks, flat or globe. Each shows its name, its direction and its fixes. Turn them on in Map options, separately from the Pacific tracks | Partial | Tracking, maps and flight pages |
| Pacific tracks | The Pacific tracks in force now, westbound from Oakland and eastbound from Fukuoka, on the Tracking, flight history, public flight and Operations maps, flat or globe. Each shows its name, its direction and its fixes. Turn them on in Map options | Partial | Tracking, maps and flight pages |
| Fix card | Click a fix on the Tracking or flight history map to read its plan: position, FIR, fuel and the wind at each level. Arrows step along the route and centre the map on each fix | Live | Tracking, maps and flight pages |
| Briefing and Tracking views | Your briefing at the gate and a full-page live map from off-block on, each with its own link. A bar under both holds the cabin, cargo and records documents and the emergency and diversion buttons | Live | Tracking, maps and flight pages |
| Predicted times | Type in the times your FMC shows or ATC gave you; they replace your estimates across the flight and on the public map | Live | — |
| Pilots in the air now | Public map of ongoing flights | Live | Tracking, maps and flight pages |
| Public flight page | Share a flight by link; map with a data panel, the live position while it flies and the whole path once it has landed | Live | Tracking, maps and flight pages |
| Flight visibility | Public, link-only or hidden | Live | Tracking, maps and flight pages |
| Flight history | Every flight you finished, on its map: the track flown over the filed route, a vertical profile against the planned altitude, each block time against schedule and the fuel burned against the planned trip, with the briefing it was flown on one click away. The list sits beside a globe of all your flights in grey, loads ten at a time and lights the flight you point at in indigo; opening one flies the globe to it | Live | Tracking, maps and flight pages |
| Live updates | Lifecycle events pushed to open screens | Live | Tracking, maps and flight pages |
| Emergencies | Declare, update and resolve an emergency | Live | Emergencies |
| Diversions | Divert, ending the flight at the diversion airport | Live | Emergencies |
| Delays | Allocate a late off-block to delay codes | Live | — |
| Discord rich presence | Show your flight as your Discord activity | Live | — |
| Pilot dashboard | Your current flight on a globe, or between flights the airport and stand you are at and your time on the ground; your next flight, your last three, each one click from its flight history, and your last 7 days in block time | Live | — |
| Current flight summary | Richer current-flight card on the dashboard | Planned | — |

## Pilot profile and collectibles

| Feature | In user language | Status | Decision |
| --- | --- | --- | --- |
| Statistics | Hours, records, types, airports and a flying-days heatmap | Live | Social and public profile |
| Aircraft history | Every airframe you've captained | Live | Aircraft and airframe continuity |
| Travel log | Where you've been, including dead-head trips | Live | Aircraft and airframe continuity |
| Airframe continuity | Repositioning recorded when a tail departs from elsewhere | Partial (no points) | Aircraft and airframe continuity |
| Passport stamps | One stamp per country you land in | Partial (no screens yet) | — |
| Postcards | A postcard for every city you reach | Partial | Core spirit (overview) |
| Rotations | Multi-leg trips planned ahead and followed leg by leg | Live | — |

## Airports and data

| Feature | In user language | Status | Decision |
| --- | --- | --- | --- |
| Airport library | Look up any airport: runways, terminals, stands, gates, weather, NOTAMs | Live | Airports |
| Airports from imports | Airports in your plan are created automatically | Live | Airports |
| OpenStreetMap upgrade | Operations review and apply airport data from OSM | Live | Airports |
| Data quality grades | Find the airports that still need work | Live | Airports |
| Suggest a correction | Propose a fix to an airport, stand, gate, terminal or runway | Live | Airports |
| Review queue | Operations and admins accept or reject proposals | Live | Airports |
| Fleet and operators | Manage airlines, aircraft, hold variants | Live | Aircraft and airframe continuity |
| Aircraft from imports | A registration in your plan that isn't in the fleet is added automatically, unconfirmed until operations confirm it | Live | Aircraft and airframe continuity |
| Technical logbook | Report and triage aircraft defects | Planned | Emergencies (MEL) |
| Loadsheet history | Keep every loadsheet revision and let the crew accept the last | Partial | — |

## Accounts

| Feature | In user language | Status | Decision |
| --- | --- | --- | --- |
| Email and password sign-in | Sign in with your email and password | Live | — |
| Google sign-in | Sign in with Google once linked | Live | — |
| Discord sign-in and link | Sign in with Discord once linked; join the server while linking | Live | — |
| Password reset and change | Reset by email link; change from your account page | Live | Notifications and comms |
| Email change | Move your account to a new address, confirmed by email | Live | Notifications and comms |
| Discord message switches | Turn each Discord DM on or off | Live | Notifications and comms |
| Transponder sign-in in the browser | Sign in to the Transponder by approving it in the browser, without typing a password | Live | Transponder |
| Signed-in sessions | See every browser and Transponder you're signed in on, and sign any of them out | Live | — |
| GSX switches | Turn the GSX integration on or off, and choose whether boarding and deboarding start by themselves when you request them through GSX | Live | Transponder |

## Platform

| Feature | In user language | Status | Decision |
| --- | --- | --- | --- |
| Installable app | Add MyPreflight to your home screen as an app | Live | Flight monitoring alerts (PWA only) |
| App updates | When a new version is ready, the installed app says so: Reload switches to it, What's new opens the changelog on the website. The sidebar shows the version you're on, with the same link | Live | — |
| Account menu | Settings, the Discord community, the theme and Sign out behind the button with your initials, top right on a tablet or computer | Live | — |
| Light and dark | Follows your system theme, or pick Light or Dark in the account menu | Live | Accessibility and theming |
| Website | mypreflight.io marketing page with FAQ | Live | — |

## In the brief, not built yet

None of these is built. Don't advertise them.

| Feature | Brief section |
| --- | --- |
| Sign-up without an admin (email, Google or Discord) | `../product/accounts-and-privacy.md` |
| Self-serve account deletion, email-hash and IP bans | `../product/accounts-and-privacy.md` |
| Flight-progress emails and email preference switches | `../product/decisions.md` → Notifications |
| Points, bonuses, penalties, repositioning fee, leaderboard | `../product/scoring.md` |
| Auto-finish of stale flights after about 24 hours | `../product/flight-lifecycle.md` |
| Plain-language METAR and TAF with icons | `../product/decisions.md` → Weather |
| A map of all of a pilot's past flights together | `../product/overview.md` → day one |
| In-flight fuel trend warnings | `../product/flight-lifecycle.md` → Live view |
| Mid-flight monitoring alerts (sim crash, tracking lost) | `../product/decisions.md` → Flight monitoring alerts |
| Web Push notifications | `../product/decisions.md` → Flight monitoring alerts |
| GSX Pro passenger counts | `../product/transponder.md` |
| MSFS 2020 support in the Transponder (it reads MSFS 2024 only) | `../product/overview.md` |
| Transponder download page | `../product/transponder.md` |
| Public pilot profiles, following, view counts | `../product/decisions.md` → Social |
| In-app "What's new" and a public changelog | `../product/brand/marketing.md` |
| "For flight simulation use only" label in the app | `../product/overview.md` → Hard lines |
| Creating unknown aircraft from open data | `../product/decisions.md` → Aircraft |
| Payments and paid features | `../product/business.md` |
