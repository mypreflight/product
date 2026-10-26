# Feature catalogue

Every MyPreflight feature, its build status and the code that proves it. Generated from the code repos, not from the brief.

Last reviewed: 2026-10-20

## How to read this

| Status | Meaning |
| --- | --- |
| Live | Built end to end and reachable in the app, the Transponder or the website |
| Partial | Some layers exist (e.g. API without screens, or groundwork without the user-facing part) |
| Planned | Backed by an open issue or an OpenSpec change in a code repo, not built yet |

Repos are named by their short name from `../repos.md`: `app`, `api`, `transponder`, `adsb`, `osm`, `aerolopa`, `postcard`, `landing`. API paths are under `src/modules/` unless stated; app paths under `app/`. "Decision" points to the matching section of `../product/decisions.md`.

Before advertising anything, check its status here. Only **Live** features go in marketing: advertising something unbuilt breaks the "Trust first" hard line in `../product/overview.md`.

## Briefing

| Feature | In user language | Status | Evidence | Decision |
| --- | --- | --- | --- | --- |
| SimBrief import | Create a flight from your latest SimBrief plan | Live | api `flights/…/flight/create-flight-from-simbrief.action.ts`; app `routes/operations/flights/CreateFlightRoute.tsx` | SimBrief import |
| OFP viewer | Read the full SimBrief OFP in the app | Live | api `flights/…/ofp/get-ofp.action.ts`; app `FlightOfpTab` | SimBrief import |
| Runway analysis | SimBrief's runway analysis page, in its own tab | Live | app `features/flight/components/Dashboard/Tabs/Tab/FlightRunwayAnalysisTab.tsx` | SimBrief import |
| Loadsheets | Preliminary revisions and a final loadsheet, never overwritten | Live | api `flights/…/loadsheet/`; app `features/flight/components/FuelAndLoadsheet/` | Passengers and cargo |
| Fuel ladder | Fuel build-up from taxi to total, with tank gauge | Live | app `features/flight/components/FuelAndLoadsheet/FuelPlan.tsx` | SimBrief import |
| Passenger manifest | Named passengers seated in the real cabin, with no-shows | Live | api `manifest/` (`get-flight-manifest`); app `FlightCabinTab`, `routes/operations/flights/FlightManifestRoute.tsx` | Passengers and cargo |
| Special assistance | Infants, wheelchairs, unaccompanied minors, by real IATA codes | Live | api `manifest/` (special services) | Passengers and cargo |
| Seat maps | The airline's real seat map, seat by seat | Live | api `cabin-layouts/`; aerolopa; app `features/cabin-layout/` | Passengers and cargo |
| Cabin layout assignment | Operations pick the layout each aircraft flies | Live | api `aircraft/…/cabin-layout/`; app `routes/operations/operators/aircraft/AircraftSeatLayoutTab.tsx` | Passengers and cargo |
| Cargo manifest | Shipments with waybills, origins and onward connections | Live | api `manifest/` (`get-flight-cargo-manifest`); app `features/cargo-manifest/` | Passengers and cargo |
| Origin-aware cargo | Tulips from Amsterdam, salmon from Keflavik, in season | Live | api `manifest/data/` (commodity catalogue) | Passengers and cargo |
| Cargo in the hold | Containers and pallets drawn in the aircraft's real hold | Live | api `manifest/data/` (hold layouts); app `features/cargo-hold/`, `AircraftHoldLayoutTab.tsx` | Passengers and cargo |
| Cold chain | Will the salmon stay cold? Risk with a plain-language reason | Live | api `manifest/` (cold chain); app `features/cargo-manifest/` | Passengers and cargo |
| NOTOC | Notification to captain with dangerous goods and drills, printable | Live | api `manifest/` (`get-flight-notoc`); app `features/notoc/` | Passengers and cargo |
| Baggage | Bags derived from the payload, loaded with the cargo | Live | api `manifest/` (baggage) | Passengers and cargo |
| Crew roster | First officer, purser and cabin crew from SimBrief | Live | api `crew/` | SimBrief import |
| Weather | Raw ATIS, METAR and TAF from two sources, your choice | Live | api `airports/…/weather/`; app `features/airport/components/Library/AirportWeatherPanel.tsx` | Weather and NOTAMs |
| NOTAMs | The NOTAMs from your OFP, per airport | Live | api `airports/…/notam/`; app `routes/pilot/airports/AirportNotamsTab.tsx` | Weather and NOTAMs |
| Gate, stand and runway | Change gate, stand and departure or arrival runway, like an EFB | Live | api `flights/…/parking-position/`, `flights/…/runway/`; app `ParkingPositionRunwayBox.tsx` | Airports |
| Planned route | The route drawn fix by fix | Partial | api `flights/…/route/get-planned-route.action.ts`; [api#289](https://github.com/oskarbarcz/flight-tracker-api/issues/289), [api#280](https://github.com/oskarbarcz/flight-tracker-api/issues/280) in progress | Tracking, maps and flight pages |
| ETOPS briefing | Entry, exit and equal-time points with diversion airports | Partial | api `flights/…/etops/`; app `features/route/components/Etops/`; OpenSpec change `add-etops-briefing`; [api#293](https://github.com/oskarbarcz/flight-tracker-api/issues/293) | — |
| Oceanic tracks | The North Atlantic tracks your flight was planned against | Partial | api `flights/…/oceanic/`; [api#291](https://github.com/oskarbarcz/flight-tracker-api/issues/291) | — |
| Discord briefing | Your briefing and loadsheets as Discord DMs | Live | api `flights/application/event/internal/send-*.listener.ts`; `src/core/provider/discord/` | Discord bot |

## Flying and tracking

| Feature | In user language | Status | Evidence | Decision |
| --- | --- | --- | --- | --- |
| Flight progress | Check in, board, off-block, takeoff, landing, on-block, close | Live | api `flights/…/lifecycle/`; app `features/flight/components/Dashboard/Tracking/FlightProgressControl/` | Flight lifecycle |
| Automatic off-block | Detected from ground speed above 3 kt | Live | api `flights/application/event/internal/detect-off-block.listener.ts` | Flight lifecycle |
| Automatic takeoff | Detected when you leave the departure airport's boundary | Live | api `…/detect-takeoff.listener.ts` | Flight lifecycle |
| Automatic landing | Detected inside the destination boundary below 50 kt | Live | api `…/detect-arrival.listener.ts` | Flight lifecycle |
| Actual fuel burn | Type the fuel you burned when you close the flight | Live | app `flight-closure` (OpenSpec); api `flights/…/lifecycle/close-flight.action.ts` | Flight lifecycle |
| Transponder | One portable .exe that feeds your position from MSFS 2024 | Live | transponder `src/infrastructure/sim/`, `src/infrastructure/adsb/` | Transponder |
| Auto standby | The Transponder switches on at boarding and off at on-block | Live | transponder `src/domain/` | Transponder |
| Transponder updates | One key saves the new version to Downloads, checksum verified | Live | transponder `src/application/updater.ts`, OpenSpec `release-updates` | Transponder |
| Live map tracking | Your aircraft moving on the map with live telemetry | Live | adsb; api `src/core/provider/adsb/`; app `features/adsb/`, `features/flight/components/Map/` | Tracking, maps and flight pages |
| Briefing and Monitor views | Your flight data at the gate, a full-width live map from off-block to on-block, switching by itself | Live | app `features/flight/lib/trackingView.ts`, `features/flight/hooks/useTrackingView.ts` | Tracking, maps and flight pages |
| Predicted times | Arrival and on-block predictions used across the flight page | Partial (nothing sends them yet) | api `flights/…/timesheet/update-predicted-timesheet.action.ts`; app `features/flight/lib/blockEvents.ts` | — |
| Pilots in the air now | Public map of ongoing flights | Live | app `routes/public/MapIndexRoute.tsx` | Tracking, maps and flight pages |
| Public flight page | Share a flight by link; map with a data panel | Live | app `routes/public/MapRoute.tsx`; api `flights/…/flight/get-flight.action.ts` | Tracking, maps and flight pages |
| Flight visibility | Public, link-only or hidden | Live | api `flights/…/tracking/change-flight-visibility.action.ts` | Tracking, maps and flight pages |
| Flight history | Every finished flight with its track on a map | Live | app `routes/pilot/history/FlightHistory*.tsx`, `features/flight/components/Dashboard/History/` | Tracking, maps and flight pages |
| Live event stream | Lifecycle events pushed to open screens | Live | api `docs/WEBSOCKETS.md`; app `socket.io-client` | Tracking, maps and flight pages |
| Emergencies | Declare, update and resolve an emergency | Live | api `flights/…/emergency/`; app `features/emergency/` | Emergencies |
| Diversions | Divert, ending the flight at the diversion airport | Live | api `flights/…/diversion/`; app `features/diversion/` | Emergencies |
| Delays | Allocate a late off-block to delay codes | Live | api `flights/…/delay/`, `automations/`; app `features/delay/` | — |
| Discord rich presence | Show your flight as your Discord activity | Live | api `users/…/get-discord-presence`; transponder `src/infrastructure/discord/` | — |
| Current flight summary | Richer current-flight card on the dashboard | Planned | [app#294](https://github.com/oskarbarcz/flight-tracker-app/issues/294) | — |

## Pilot profile and collectibles

| Feature | In user language | Status | Evidence | Decision |
| --- | --- | --- | --- | --- |
| Statistics | Hours, records, types, airports and a flying-days heatmap | Live | api `statistics/`; app `routes/pilot/stats/PilotStatsRoute.tsx` | Social and public profile |
| Aircraft history | Every airframe you've captained | Live | api `users/…/get-my-aircraft`; app `routes/pilot/history/AircraftHistory*.tsx` | Aircraft and airframe continuity |
| Travel log | Where you've been, including dead-head trips | Live | api `users/…/travel`; app `routes/pilot/travels/TravelLogRoute.tsx` | Aircraft and airframe continuity |
| Airframe continuity | Repositioning recorded when a tail departs from elsewhere | Partial (no points) | api `aircraft/application/event/external/reposition-flight-lifecycle.listener.ts` | Aircraft and airframe continuity |
| Passport stamps | One stamp per country you land in | Partial (API only) | api `statistics/…/get-my-countries`, `get-my-country-stamps` | — |
| Postcards | A postcard for every city you reach | Partial | api `game/`; postcard; app `routes/pilot/postcards/`, `routes/operations/postcards/`; [app#292](https://github.com/oskarbarcz/flight-tracker-app/issues/292), [app#289](https://github.com/oskarbarcz/flight-tracker-app/issues/289) in progress; OpenSpec change `add-city-postcards` | Core spirit (overview) |
| Rotations | Multi-leg trips planned ahead and followed leg by leg | Live | api `rotations/`; app `features/rotation/`, `routes/pilot/rotations/` | — |

## Airports and data

| Feature | In user language | Status | Evidence | Decision |
| --- | --- | --- | --- | --- |
| Airport library | Look up any airport: runways, terminals, stands, gates, weather, NOTAMs | Live | app `routes/pilot/airports/` | Airports |
| Airports from imports | Airports in your plan are created automatically | Live | api `airports/application/command/import-airport-by-icao.command.ts` | Airports |
| OpenStreetMap upgrade | Operations review and apply airport data from OSM | Live | api `airports/…/enrich/`; osm; app `features/airport/components/Enrichment/` | Airports |
| Data quality grades | Find the airports that still need work | Live | api `airports/` (data quality); app `routes/operations/airports/AirportsListRoute.tsx` | Airports |
| Suggest a correction | Propose a fix to an airport, stand, gate, terminal or runway | Live | api `change-requests/`; app `features/change-request/` | Airports |
| Review queue | Operations and admins accept or reject proposals | Live | api `change-requests/…/review/`; app `routes/review/data-changes/` | Airports |
| Fleet and operators | Manage airlines, aircraft, hold variants | Live | api `operators/`, `aircraft/`; app `routes/operations/operators/` | Aircraft and airframe continuity |
| Technical logbook | Report and triage aircraft defects | Planned | OpenSpec change `add-aircraft-technical-logbook`; api#151–157 | Emergencies (MEL) |
| Loadsheet history | Keep every loadsheet revision and let the crew accept the last | Partial | [api#286](https://github.com/oskarbarcz/flight-tracker-api/issues/286); OpenSpec change `extract-flight-loadsheets` | — |

## Accounts

| Feature | In user language | Status | Evidence | Decision |
| --- | --- | --- | --- | --- |
| Email and password sign-in | Sign in with your email and password | Live | api `auth/`; app `routes/auth/SignInRoute.tsx` | — |
| Google sign-in | Sign in with Google once linked | Live | api `auth/` (`google-sign-in`, `link-google-account`); app `features/auth/` | — |
| Discord sign-in and link | Sign in with Discord once linked; join the server while linking | Live | api `auth/` (`discord-sign-in`, `link-discord-account`); app `features/auth/` | — |
| Password reset and change | Reset by email link; change from your account page | Live | api `users/`; app `routes/common/AccountRoute.tsx` | Notifications and comms |
| Email change | Move your account to a new address, confirmed by email | Live | api `users/` (`request-email-change`); app `routes/common/AccountRoute.tsx` | Notifications and comms |
| Discord message switches | Turn each Discord DM on or off | Live | api `users/…/discord-settings`; app `routes/common/AccountRoute.tsx` | Notifications and comms |
| Transponder sign-in in the browser | Sign in to the Transponder without typing a password | Planned | OpenSpec change `add-device-code-flow` (api) | Transponder |

## Platform

| Feature | In user language | Status | Evidence | Decision |
| --- | --- | --- | --- | --- |
| Installable app | Add MyPreflight to your home screen as an app | Live | app `vite-plugin-pwa`, `public/splash/` | Flight monitoring alerts (PWA only) |
| Light and dark | Follows your system theme | Live | app `root.tsx` (`ThemeModeScript`) | Accessibility and theming |
| Website | mypreflight.io marketing page with FAQ | Live | landing `src/pages/index.astro` | — |

## In the brief, not found in code

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
