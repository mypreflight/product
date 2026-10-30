# Feature catalogue

Every MyPreflight feature and its build status, taken from what's actually built, not from the brief.

Last reviewed: 2026-10-20

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
| SimBrief import | Create a flight from your latest SimBrief plan | Live | SimBrief import |
| OFP viewer | Read the full SimBrief OFP in the app | Live | SimBrief import |
| Runway analysis | SimBrief's runway analysis page, in its own tab | Live | SimBrief import |
| Loadsheets | Preliminary revisions and a final loadsheet, never overwritten | Live | Passengers and cargo |
| Fuel ladder | Fuel build-up from taxi to total, with tank gauge | Live | SimBrief import |
| Passenger manifest | Named passengers seated in the real cabin, with no-shows | Live | Passengers and cargo |
| Special assistance | Infants, wheelchairs, unaccompanied minors, by real IATA codes | Live | Passengers and cargo |
| Seat maps | The airline's real seat map, seat by seat | Live | Passengers and cargo |
| Cabin layout assignment | Operations pick the layout each aircraft flies | Live | Passengers and cargo |
| Cargo manifest | Shipments with waybills, origins and onward connections | Live | Passengers and cargo |
| Origin-aware cargo | Tulips from Amsterdam, salmon from Keflavik, in season | Live | Passengers and cargo |
| Cargo in the hold | Containers and pallets drawn in the aircraft's real hold | Live | Passengers and cargo |
| Cold chain | Will the salmon stay cold? Risk with a plain-language reason | Live | Passengers and cargo |
| NOTOC | Notification to captain with dangerous goods and drills, printable | Live | Passengers and cargo |
| Baggage | Bags derived from the payload, loaded with the cargo | Live | Passengers and cargo |
| Crew roster | First officer, purser and cabin crew from SimBrief | Live | SimBrief import |
| Weather | Raw ATIS, METAR and TAF from two sources, your choice | Live | Weather and NOTAMs |
| NOTAMs | The NOTAMs from your OFP, per airport | Live | Weather and NOTAMs |
| Gate, stand and runway | Change gate, stand and departure or arrival runway, like an EFB | Live | Airports |
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
| Transponder | One portable .exe that feeds your position from MSFS 2024 | Live | Transponder |
| Auto standby | The Transponder switches on at boarding and off at on-block | Live | Transponder |
| Transponder updates | One key saves the new version to Downloads, checksum verified | Live | Transponder |
| Live map tracking | Your aircraft moving on the map with live telemetry | Live | Tracking, maps and flight pages |
| Briefing and Tracking views | Your flight data at the gate and a full-page live map from off-block on, each with its own link | Live | Tracking, maps and flight pages |
| Predicted times | Arrival and on-block predictions used across the flight page | Partial (nothing sends them yet) | — |
| Pilots in the air now | Public map of ongoing flights | Live | Tracking, maps and flight pages |
| Public flight page | Share a flight by link; map with a data panel | Live | Tracking, maps and flight pages |
| Flight visibility | Public, link-only or hidden | Live | Tracking, maps and flight pages |
| Flight history | Every finished flight with its track on a map | Live | Tracking, maps and flight pages |
| Live updates | Lifecycle events pushed to open screens | Live | Tracking, maps and flight pages |
| Emergencies | Declare, update and resolve an emergency | Live | Emergencies |
| Diversions | Divert, ending the flight at the diversion airport | Live | Emergencies |
| Delays | Allocate a late off-block to delay codes | Live | — |
| Discord rich presence | Show your flight as your Discord activity | Live | — |
| Pilot dashboard | Your current flight with a route map, your next and last flight, and your last 7 days in block time | Live | — |
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
| Transponder sign-in in the browser | Sign in to the Transponder without typing a password | Planned | Transponder |

## Platform

| Feature | In user language | Status | Decision |
| --- | --- | --- | --- |
| Installable app | Add MyPreflight to your home screen as an app | Live | Flight monitoring alerts (PWA only) |
| Light and dark | Follows your system theme | Live | Accessibility and theming |
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
| In-flight fuel checks at waypoints and trend warnings | `../product/flight-lifecycle.md` → Live view |
| Mid-flight monitoring alerts (sim crash, tracking lost) | `../product/decisions.md` → Flight monitoring alerts |
| Web Push notifications | `../product/decisions.md` → Flight monitoring alerts |
| GSX Pro passenger counts | `../product/transponder.md` |
| MSFS 2020 support in the Transponder (it reads MSFS 2024 only) | `../product/overview.md` |
| Transponder download page | `../product/transponder.md` |
| Public pilot profiles, following, view counts | `../product/decisions.md` → Social |
| In-app "What's new" and a public changelog | `../product/brand/marketing.md` |
| "For flight simulation use only" label in the app | `../product/overview.md` → Hard lines |
| Automatic creation of unknown aircraft | `../product/decisions.md` → Aircraft |
| Payments and paid features | `../product/business.md` |
