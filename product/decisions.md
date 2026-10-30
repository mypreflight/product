# Product decisions

Current product decisions, grouped by area, with the reason and Oskar's examples. Everything here is decided unless marked **Open:**.

Last reviewed: 2026-10-20

## SimBrief import and briefing

- Import pulls essentially every field the SimBrief API offers; not all are used yet. The OFP can be previewed in the app.
- From SimBrief we take passenger count, payload, cargo weights, scheduled times, departure runway and registration. Everything else is generated on the platform: names, infants, PNR records, cargo contents.
- Fuel is shown in tonnes **and** in minutes, far more clearly than SimBrief shows it.
- SimBrief registrations are user-entered and not checked against the aircraft type, so they can't be trusted on their own.
- An imported plan never carries more passengers than the aircraft's seat map: the count is capped at the seats. SimBrief plans against a generic cabin, while the seat map is the airline's real configuration, so the import keeps working instead of being refused.

## Passengers and cargo

- Passenger manifests are realistic and generated: fake names, PNRs, infants, passengers with reduced mobility or wheelchair assistance, hearing impairments, unaccompanied minors. Names are fake, so no personal data is involved.
- Cargo is generated deterministically from the origin's context, never at random: tulips from Amsterdam, salmon from Keflavik, iPhones and other electronics from manufacturing hubs. Cargo means freight under the belly, distinct from passenger baggage.
- Loads follow the airline: cargo airlines carry cargo only; passenger airlines carry passengers plus cargo according to their schema.
- Seat maps come from AeroLOPA, with exact layouts for about 80% of the most popular airlines, seats mapped to business and economy classes.
  - Fallback order: parent or similar airline layout (e.g. Lufthansa City uses the Lufthansa A321), then a generic type layout, with a notice that it's a close match, not exact.
  - Capacity follows the airline's real configuration: a Ryanair 737 MAX 8 carries more passengers than a LOT one.
- Loadsheets are loosely modelled on Lufthansa's. There's no industry standard; Lufthansa itself uses about five formats across its systems. Airline-specific layouts (the way SimBrief offers LIDO-style per-airline flight plans) would need format input and are low value for now.
- **Open:** confirm AeroLOPA's licence terms for our use.

## Aircraft and airframe continuity

- Pilots select a real airframe by registration (e.g. SP-LRC, a LOT 787-9), not an add-on aircraft type.
- Only real aircraft, current or historic, with real markings. Historic airlines such as Pan Am and US Airways count; so does a KLM 747 for the Maho Beach approach.
- The sim livery and callsign don't have to match the selected registration. The Transponder does nothing on a mismatch.
- Each pilot has a per-airframe history, so continuity matters. If a tail's next departure isn't where it last landed (landed in Zurich, next flight departs Frankfurt), the pilot pays a points fee for the implied repositioning flight, because that ferry flight is a loss for the airline. Realistic continuity is rewarded.
- Missing registrations are requested through the data-request feature. This is a known weak spot.
- Today the import fails with an error if the selected aircraft doesn't exist. Direction: create unknown aircraft automatically from open data, the way unknown airports are created from OpenStreetMap.
- **Open:** research the OpenSky Network aircraft database as that source.

## Airports

- Airport data comes from OpenStreetMap, attributed, and is kept fresh by a dedicated automated updater.
- Gates (passenger side) and parking stands (aircraft and crew side) are stored separately. The terminal is derived from them and isn't editable. Examples: Warsaw gate 17 sits in Terminal A; Frankfurt stand 225 belongs to Terminal 1.
- Pilots can change gate, stand and departure runway, as on a real EFB. The runway comes from SimBrief and can be updated, e.g. when the wind changes. Bonus points when the actual takeoff runway matches the selection.
- Noise abatement procedures and curfews (e.g. Heathrow, Munich) are entered by hand today, which is painful, and no open database or API for them has turned up. After the first release we build an **AIP analyzer**: AI-assisted extraction from national AIPs with human sourcing and review.
  - European AIPs are fairly uniform; others, such as Colombia's or Mexico's, are messy.
  - Top ~50 airports first, ~1,000 as the target.
  - Token cost must stay under control.
- Users suggest data corrections or additions (e.g. a missing airline) through an in-app button. The team reviews them; users can't edit data directly. Oskar reviews them himself for now; role management can grant review rights to other accounts.

## Weather and NOTAMs

- METAR and TAF are translated deterministically into plain language and icons (temperature, pressure and so on), with a toggle to the raw text.
- NOTAMs are imported raw (formatted from the SimBrief OFP) at import time only, so they can be 6 to 8 hours old when the flight arrives.
- Plain-language NOTAM translation is wanted: "RWY 14 CLSD" becomes "Runway 14 is closed". NOTAMs are free-form human shorthand, so deterministic rules can't cover them all; it needs AI and would be a paid add-on because of token cost. It's core to what pilots brief, so its priority may rise.
- No free NOTAM API has been found. The FAA NOTAM API is US-centric and limited for Europe.
- **Open:** a free NOTAM source, a free METAR source, and how high to prioritise NOTAM translation.

## SIDs, STARs and procedures

- No SIDs or STARs before launch. They add a few map points for a lot of development; it's a post-launch feature request.
- Why it's hard: SimBrief gives a SID and departure runway at import, but on VATSIM or in changing weather the runway, and so the SID, often changes on the go.
- No points for changed SIDs. We only know SIDs from imported data and can't detect a change unless the flight is re-imported.
- A direct line from the airport to the first route fix is better than showing an incorrect SID.
- FAA CIFP (free US procedure data: SIDs, STARs, approaches) has raised priority and is wanted sooner, through an API if one exists.

## Tracking, maps and flight pages

- Flight visibility:

  | Mode | Who sees the flight |
  | --- | --- |
  | Private | Only the signed-in pilot |
  | Link-only | Anyone with the link |
  | Public | Everyone; it appears on the public map |

  Default is private today and will change to public, with a UI component explaining the options.
- Public flight page: flight-tracker layout with the map in the centre and a data panel overlaid on the left. Shows departure and arrival airports, scheduled vs actual off-block (the distinction matters), and current position. Great-circle line and the route through flight-plan fixes are in progress. Wanted: richer route and flight content, maybe the cargo.
- Visitors see everything on a public flight. Some resources still don't show to visitors; that's a known bug.
- Map flight history is the flagship feature. It's live for beta users and looks good; it must load faster and become outstanding.
- A live "pilots in the air now" view exists.

## Social and public profile

- Pilots will be able to follow each other (not designed yet).
- No likes on flights or landings. Show view counts instead: how many people opened the flight or saw it on the map.
- Public pilot info: name and surname, hours flown (already collected), types flown, stats. Profiles are private today; shareable public profiles with aggregated history (aircraft types, registrations) are planned, maybe not for launch.
- **Open:** profile pictures. Leaning no: uploads mean moderating spam and ad images.
- **Open:** display names and nicknames. Only acceptable with moderation: blocklists for scam and spam domains, forbidden words and swearing, and no offensive or meme names (e.g. Polish Pope jokes). The platform should feel open and welcoming. Nicknames must also not be confusable with registrations (a pilot called "SPLRC" looks like the aircraft SP-LRC).

## Notifications and comms

- Discord first for news and announcements: cheapest and easiest.
- Two email types, both already in the terms:
  - mandatory transactional emails (ban, password recovery and similar);
  - flight-progress updates such as "you haven't finished your flight, something may be off". This kind is likely to come because it's easy to automate.
- No newsletter for now; making it look good is too much effort.
- Email preference switches and opt-ins already exist.

## Flight monitoring alerts

- The Transponder will detect anomalies mid-flight: an abrupt vertical speed change (the cat walked over the joystick), a sim crash to desktop, tracking lost. The pilot gets alerted fast, so they can reconnect or reload the aircraft autosave while the gap is minutes, not hours.
- Why it matters: long-haul flights are often flown in real time, with the pilot away doing chores or groceries during cruise over the Atlantic.
- Alert channels, in order:
  1. **Discord**, written as a message from dispatch ("Hey, what's going on?").
  2. **Web Push** on the PWA. iOS 16.4+ supports it for apps added to the home screen; to be tested on an iPhone, with friends testing Android.
  3. **SMS** last, if ever, because it costs money and work.
- No native apps. PWA only. Native would mean store fees (Google about $25 once, Apple about $99 a year), separate apps to build and app reviews; far future at most.

## Discord bot

- One-way, like a real flight dispatcher: it delivers manifests and loadsheets as messages.
- No AI two-way chat: token cost, prompt-injection and token-burning abuse, and little value beyond what the UI already does.

## Emergencies

- Pilot-declared through the app (declare emergency or malfunction), never auto-detected, as in real life. Auto-detection would need internal MSFS APIs and would be hard to port to X-Plane.
- MEL items can be labelled; MEL management is partly built and not a priority.
- Diversions are supported, e.g. a medical emergency with a healthy aircraft. A diversion ends the flight at the diversion airport; landing is final. The pilot then creates an onward flight with the passengers or a ferry flight.
- Never penalised. Only handling is judged. A hard emergency landing just earns no bonus. No points effect yet. Emergencies are a nice addition, not a main feature.

## Accessibility and theming

- No dedicated colour-blind mode: the cost to build and maintain it isn't worth it now, and the current UI has been checked.
- Warnings and errors are never colour-only: bold text, icons, modals. Few components rely on red versus green.
- Light and dark themes follow the operating system automatically.

## Licence

- Currently The Unlicense (public domain). Switch to a protective licence before users and contributors arrive, after a quick legal check.
- The aim is to stop someone copying MyPreflight and rebranding it as a competing platform. The risk is very low but not zero.
- **Open:** which licence.
