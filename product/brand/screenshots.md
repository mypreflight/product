# Screenshots

Which screens to capture for which feature, in what state.

Last reviewed: 2026-10-20

## Rules

- Use real-looking flights, never empty states: a long-haul flight with salmon in the hold, a full manifest with special-assistance passengers, a map with a long flown track.
- Capture on phone or tablet size where the feature lives there; the app is a second screen.
- Show both themes across a set; don't mix themes within one post.
- No real users' names or data in screenshots unless they agreed. The demo data (fake pilots, fake passengers) is safe.
- Never fake the UI or invent numbers. If the demo data makes a panel look broken, fix the data and let the app render it.
- Only **Live** features from `../../features/catalogue.md`.
- For the website's feature slots: component crops, not full views, rendered at 1920×1080 (a 960×540 stage at 2× scale), one file per theme.

## How to get there

Run the app with its demo data. Sign in as the demo pilot or the demo operations user.

Set-up for most shots: an imported SimBrief flight on a wide-body, long-haul sector (e.g. London Heathrow to New York JFK on an A350 or 777), with a cabin layout and hold variant assigned to the aircraft, and a preliminary loadsheet written so manifests exist.

The demo flights are dated January and June 2025. Captured on today's date, every time carries a date and the countdowns run to thousands of hours. Set the browser's clock to the flight's own day instead, and say so in the caption ("Seeded demo flights, shown on the day they fly").

## Before and after graphics

Release posts about a redesign compare the previous release with the new one:

- One change per graphic, at 1920×1080: a claim headline, two or three lines of explanation, a Before and an After line, then the two crops side by side.
- Both crops are real captures. Run the previous release next to the new one against the same demo data, with the same clock, theme and viewport.
- A dashed amber ring marks what changed in the Before crop, a solid indigo ring in the After crop.
- When a crop shows a value typed for the shot, the footer says so, and that nothing was submitted. Open dialogs and cancel; never move a demo flight on.
- Alternate light and dark across the set.

## Shot list

| Feature | View | State to set up | Device | Theme |
| --- | --- | --- | --- | --- |
| Passenger manifest | Flight page, Briefing, passengers tab | Seated wide-body, a few SSR passengers (infant, wheelchair, unaccompanied minor), a couple of no-shows after finish-boarding | Tablet | Light |
| Seat maps | Flight page, Briefing, passengers tab (cabin drawing) | Layout whose geometry matches; premium cabins visible | Tablet | Dark |
| Cargo manifest | Flight page, Briefing, cargo tab | Belly cargo with a perishable shipment (salmon out of Keflavik, or tulips out of Amsterdam in season), containers in positions | Tablet | Light |
| Cold chain | Flight page, Briefing, cargo tab (advisories) | A temperature-controlled shipment with a connection, so the risk reason reads in full | Phone | Dark |
| Cargo in the hold | Flight page, Briefing, cargo tab (hold drawing) | Containerised wide-body, some positions empty | Desktop | Light |
| NOTOC | Finish-boarding modal (final loadsheet), and its document view | At least one dangerous goods consignment plus a notifiable load | Desktop | Light |
| Fuel ladder | Flight page, Briefing, fuel and crew tab | SimBrief fuel breakdown with an ETOPS or extra-fuel addition | Phone | Light |
| Loadsheet | Flight page, Briefing, fuel and crew tab | Preliminary and final both present | Tablet | Dark |
| OFP viewer | Flight page, Briefing, OFP tab | SimBrief OFP, first page visible | Tablet | Light |
| Runway analysis | Flight page, Briefing, runway analysis tab | SimBrief flight | Tablet | Dark |
| Flight progress | Flight page, Briefing (phase card: lifecycle track and next-step panel) | Boarding finished or taxiing out, automatic detection available | Phone | Dark |
| Automatic detection | Flight page, Briefing, progress tab | After takeoff, with automatic off-block and takeoff shown in the timeline | Tablet | Light |
| Live map tracking | Flight page, Tracking | In cruise with the whole route fitted below the cards and live telemetry in the bottom bar | Tablet | Dark |
| Pilots in the air now | Public map | Several ongoing public flights | Desktop | Dark |
| Public flight page | Public map, one flight | A public flight in cruise, scheduled vs actual off-block visible | Phone | Light |
| Flight history | Flight history, one flight | Closed long-haul flight with its full track | Tablet | Dark |
| Gate, stand and runway | Flight page, Briefing (stand and runway box) | Stand and runway assigned at a well-mapped airport (e.g. Frankfurt) | Phone | Light |
| Weather | Airport library, one airport (weather) | Airport monitored by an active flight; ATIS from SayIntentions | Phone | Dark |
| NOTAMs | Airport library, one airport (NOTAMs) | Airport from a recent import | Tablet | Light |
| Airport library | Airport library, one airport (parking positions) | A flagship-grade airport with terminals, stands and gates | Tablet | Dark |
| Emergencies and diversions | Flight page, Briefing, emergencies tab | A declared medical emergency with a diversion | Tablet | Light |
| Delays | Flight page, Briefing, delays tab | Late off-block, minutes allocated to codes | Phone | Light |
| Statistics | Statistics | A pilot with months of flying: heatmap filled, type breakdown | Tablet | Dark |
| Aircraft history | Aircraft history | Several airframes flown, real registrations | Phone | Light |
| Travel log | Travel log | Mix of flights and dead-head trips | Phone | Dark |
| Rotations | Rotations, one rotation | A ready rotation of three or four legs, the first one flown | Tablet | Light |
| Suggest a correction | Airport library, request-change form | A gate correction filled in | Phone | Light |
| Review queue | Data changes, one proposal (operations) | A stand proposal with current vs proposed geometry | Desktop | Dark |
| OpenStreetMap upgrade | Operations airport list, enrich review | An airport with added, updated and removed records | Desktop | Light |
| Discord briefing | A Discord DM (or the in-app sample) | Briefing with weather and the OFP attached | Desktop | Dark (Discord) |
| Discord rich presence | Discord profile | Flight in cruise, presence on | Desktop | Dark (Discord) |
| Transponder | Windows console | Signed in, `MODE C`, sim connected, Discord connected | Desktop | Console |
| Installable app | Home screen and splash on a phone | Installed PWA | Phone | Both |

## Not to capture yet

Partial and planned features (postcards, passport stamps, ETOPS briefing, oceanic tracks, planned route, technical logbook) wait until they're Live in the catalogue.
