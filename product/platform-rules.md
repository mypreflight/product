# Platform rules

The rules the built platform follows outside the briefing: roles, the flight's states, automatic detection, delays, data review, Discord, accounts and collectibles. Migrated from the backend and frontend OpenSpec specs.

Last reviewed: 2026-10-20

This file describes what is built. The decisions behind it live in `decisions.md`; where the two disagree, the decision wins and the gap is a bug or open work. Full requirement text stays in the code repos (`openspec/specs/`) and their tests.

## Roles

Each account has exactly one role.

| Role | Does |
| --- | --- |
| CabinCrew | The pilot. Checks in, boards, reports off-block, takeoff, arrival and on-block, fills the final loadsheet, closes the flight, declares emergencies and diversions, files delay reports, proposes data corrections. |
| Operations | Dispatch. Creates and imports flights (including from SimBrief), writes preliminary loadsheets, manages operators, fleet, rotations, airports and cabin layouts, decides delays, reviews data proposals. |
| Admin | Creates and edits users, reviews data proposals. |

The role name `CabinCrew` is historical: it is the pilot role.

## Flight states

`created` → `ready` → `checked_in` → `boarding_started` → `boarding_finished` → `taxiing_out` → `in_cruise` → `taxiing_in` → `on_block` → `offboarding_started` → `offboarding_finished` → `closed`

| Transition | Who | Notes |
| --- | --- | --- |
| Mark ready | Operations | Needs a preliminary loadsheet |
| Check in | Pilot | Records the pilot's estimated off-block, takeoff, arrival and on-block times; acknowledges the preliminary NOTOC; starts weather monitoring; sends the Discord briefing. The departure stand can't be set or changed after check-in; the arrival stand can until on-block. |
| Start / finish boarding | Pilot | Finish-boarding writes the final loadsheet and reconciles manifests |
| Off-block | Pilot or automatic | Automatic: first stored position above 3 kt while boarding is finished |
| Takeoff | Pilot or automatic | Automatic: first stored position outside the departure airport's boundary |
| Arrival | Pilot or automatic | Automatic: inside the destination boundary below 50 kt |
| On-block | Pilot | Not detected |
| Offboarding, close | Pilot | Close captures the actual fuel burn; blocked until any delay is settled and any emergency is resolved |

- Each automatic report happens at most once, only from its own state, and is flagged as automatic in the timeline. Detection needs an airport boundary; without one, the pilot reports manually.
- Every lifecycle event is stored and streamed to subscribers of the flight over a WebSocket.
- In the app, the pilot confirms each of these steps in a dialog before it's sent: start boarding, the off-block, takeoff, arrival and on-block reports, and start and finish offboarding. The dialog names the phase the flight moves to and warns when automatic detection is already watching for that event. Check-in, finish-boarding and close open their own forms instead, with no second confirmation.
- A reported time is the moment the pilot confirms. There's no typed time.
- While automatic detection is watching for off-block, takeoff or arrival, the app keeps the manual report behind "See details", together with the conditions detection relies on. When detection is unavailable, both show straight away, because the pilot has to report by hand.
- Cargo flights load and unload instead of boarding and offboarding. Their finished states read "Loading cargo finished" and "Unloading cargo finished".

## Tracking

- The ADS-B receiver holds live positions; the API polls it for every flight from check-in to on-block, backs up each flight's path and signals the first live position exactly once.
- The app polls live positions only after that signal.
- Visibility modes in code:

  | Mode | Anonymous visitor |
  | --- | --- |
  | `public` | Sees the flight, its path, and finds it in the public list |
  | `private` | Sees the flight by link, but not its path |
  | `disabled` | Sees nothing |

  New flights default to `private`. Operations change the mode.

- The pilot's flight page has two views, each with its own address. **Briefing** (`/track/:id/briefing`, and `/track/:id/briefing/:tab` for each data tab) shows the data tabs; **Tracking** (`/track/:id/tracking`) shows a full-page live map behind the flight and phase cards. A bare `/track/:id` opens Briefing before off-block and Tracking from off-block on, including after landing. When off-block is reported while the page is open on Briefing, it moves to Tracking by itself; otherwise the view stays where the pilot put it. Older links (`/track/:id/:tab`, `/track/:id/monitor`) redirect to their new addresses.

## Flight times

- A flight's timesheet holds up to four sets of off-block, takeoff, arrival and on-block times:

  | Set | Source |
  | --- | --- |
  | Scheduled | The plan, from Operations or the SimBrief import |
  | Estimated | The pilot, at check-in |
  | Predicted | Sent to the API while the flight runs; each time is frozen once its actual event is reported. Nothing sends predictions yet. |
  | Actual | The lifecycle reports, manual or automatic |

- Wherever the app shows an event's time it takes the first that exists: actual, predicted, estimated, scheduled. Countdowns run to that same time, so a page never shows two different arrival times.
- Scheduled and estimated times carry distinct icons (a calendar for scheduled, a timer for estimated), each with a tooltip and a screen-reader label.

## Delays

- A departure delay is raised automatically when actual off-block is 3 or more minutes after scheduled.
- The pilot allocates the minutes across delay reports with three-letter reason codes (e.g. `AMZ` for an ATC start-up delay, `ESR` for an engineering system reset). Operations accept or reject each report.
- Reports nobody decides are auto-approved after 20 to 40 minutes, fixed per request, for up to 24 hours.
- The pilot can't close the flight until the delay is settled.

## Emergencies and diversions

- The pilot declares, updates and resolves emergencies. The declaration counts souls on board from the flight's loadsheet and fills dangerous goods from the cargo manifest.
- The pilot or operations report and update a diversion.

## Aircraft and pilot continuity

- Every aircraft and every pilot has a last-known airport.
- When a flight departs from somewhere else, an automatic dead-head reposition is recorded at check-in, with its distance. Operations can also record an aircraft reposition by hand, and pilots their own travel.
- No points are attached yet.

## Rotations

- Operations plan a pilot's chain of sectors ahead of the flights: `draft` → `ready` → `in_progress` → `finished`, or `canceled` from `ready`.
- Legs are ordered by off-block time and must connect when the rotation is marked ready.
- A rotation starts on the first check-in and finishes when its last flight closes. Pilots see their released rotations read-only.

## Pilot dashboard

- **Current flight:** a map strip across the top shows the route. While the aircraft is taxiing or airborne it adds the flown track and the aircraft's position, and opening the map goes to the flight's Tracking view. Below it, the flight number, the route and the reference time share one row. Between the airports, a gauge fills for the great-circle distance flown, up to the live position, with the distance flown and to go and the block time against schedule beneath it. Before departure it shows the total distance, after arrival the distance flown; in flight without a position it shows the total and never guesses where the aircraft is. Then come the flight's phase and the pilot's next step. The card shows off-block until off-block, then on-block. A time that isn't actual is marked Est. or Sched., and carries a date when it isn't today in UTC. A missed event says how late it is ("Off-block overdue: 41m", or "overdue since 01JAN 2025" after a day).
- **Next scheduled flight:** the earliest ready flight by scheduled off-block, leaving out the current flight.
- **Last flight:** the flight with the latest actual on-block, with any delay in minutes ("+30 min late").
- **Your activity:** lifetime block time to the minute, fuel burned and distance flown, and one bar per day for the last 7 days, scaled to the busiest day.
- A card that fails to load says so and asks for a reload. It never falls back to an empty state.

## Airport data

- Airports a SimBrief import mentions but we don't hold are created automatically, at the lowest data-quality grade.
- Operations upgrade an airport from OpenStreetMap: they pull a proposal, review one change per record, and apply only what they select. A removed record is deleted only when selected.
- What enrichment never overwrites:
  - A runway's magnetic heading and lighting.
  - A curated value, when OpenStreetMap reports an `unknown` surface, a `remote` stand or no gate-to-stand link.
  - A terminal's name, taxi time, operators and notes; these are written only when the terminal is created.
- Grades are `low`, `high` and `flagship`. A completed enrichment grades the airport `flagship`; otherwise the grade is an editorial call by operations.

## Data change proposals

- Pilots propose corrections to airports, parking stands, gates, terminals and runways. Operations and admins review every kind in **one queue**, seeing current and proposed values side by side.
- A decision covers the whole proposal. The last write wins, and accepting keeps the overwritten values.
- Pilots follow and withdraw their own proposals. Several may be pending for the same record.

## Discord

| When | Message | Where |
| --- | --- | --- |
| Check-in | Flight briefing | DM to the captain |
| Boarding starts | Preliminary loadsheet | DM to the captain |
| Boarding starts | Announcement | Community server |
| Boarding finishes | Final loadsheet | DM to the captain |
| Delay raised | Request to allocate it | DM to the captain |
| Operations approve the allocation | Confirmation | DM to the captain |
| On-block | Announcement | Community server |

- Messages go to the captain, the pilot who checked in, never to whoever triggered them. A flight nobody checked in for sends no DMs.
- Each DM type has its own switch, all on by default; the two delay messages share one switch.
- **Rich presence** shows the current flight as the pilot's Discord activity through the Transponder. It's off by default, because it publishes what the pilot is doing.
- DMs only reach pilots who share the server with the bot. Linking can join the server in the same step. The account page shows membership, or "unknown" when it can't be established.
- No Discord access token is kept.

## Accounts and sign-in

- Admins create accounts. There is no public sign-up endpoint today.
- Sign-in: email and password, or Google or Discord **once linked** to an existing account. Third-party sign-in never creates an account and resolves users by provider ID only, never by email.
- Linking and unlinking Google or Discord never leaves an account without a way in.
- Passwords: changing proves the current one and signs out every other session. A reset link is single-use, expires in one hour and is never re-sent within five minutes. The reset form never reveals whether an address has an account.
- Email change: the new address must be confirmed within 24 hours; the old address is told; confirming signs out every session.
- Email is sent only for password reset and email change, in plain text. Flight messages go through Discord.
- A SimBrief user ID is verified against SimBrief before it's saved.

## Statistics and collectibles

- Statistics are recomputed from completed flights, never incremented, and count only the operating pilot. They cover lifetime totals, personal records, per type, airport and country, calendar periods in UTC (weeks run Monday to Sunday) and a flying-days heatmap.
- **Passport:** one stamp per arrival in a country; a diversion stamps where the aircraft landed.
- **Aircraft history:** every aircraft a pilot captained, recorded at check-in.
- **Postcards:** one AI-generated image per city, earned the first time a pilot lands there. Operations draw missing ones and redraw unsuitable ones. The app screens are in progress.
