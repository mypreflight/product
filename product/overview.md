# Product overview

What MyPreflight is, who it's for, and the lines it never crosses.

Last reviewed: 2026-10-20

## Pitch

SimBrief gives you static numbers: weights, fuel, route. MyPreflight gives you the real-world context around them. SimBrief says how heavy the payload is; MyPreflight says what it is: passengers with names, an infant, a wheelchair passenger, an unaccompanied minor, and salmon from Keflavik in the hold that has to stay cold. Every level of information in the sim gets richer.

## Two pillars

1. **Pre-flight briefing.** Import from SimBrief and get airline-style materials: passenger manifest, cargo, loadsheet, the airline's real seat map, plain-language weather, NOTAMs, gate, stand and runway.
2. **In-flight tracking.** The Transponder streams the flight. Events are recorded (off-block, takeoff, landing, emergencies), the history lands on a map, flights get public pages, and pilots compare hours, long-haul vs short-haul and results with other simmers.

## How it's used

A second-screen companion. The pilot flies MSFS, not the app; MyPreflight runs beside them on a tablet or phone as a PWA, the way a real crew briefs from an EFB tablet. It works well on phones and tablets.

Long-term vision: one browser tab for everything a sim pilot needs around the flight (briefing, weather, tracking, maybe charts and VA integration one day).

## Audience

- **Target:** frequent simmers in between casual and hardcore. They like virtual-airline realism but dislike VA restrictions such as fixed fleets and routes. With MyPreflight they switch freely: Lufthansa today, a cargo run tomorrow, a business jet at the weekend.
- **Hardcore VA pilots** already get some of this data from their VA, with less depth.
- **Assumed:** they already use SimBrief. Non-SimBrief users are not the target; at most we point them to tutorials.
- **Not target:** GA, helicopter and military flyers. See `scope.md`.

## What impresses on day one

Map flight history (it looks beautiful), the fact that it's free, and the passenger and cargo detail.

## Edge

Built by a licensed pilot who knows real pilot documents and airline paperwork: NOTAMs, loadsheets, OFPs, dispatch.

## Core spirit

AI speeds up our development, but the product is the opposite of AI-generated. Passengers, cargo, routes and connections are deterministic and built on open or properly licensed aviation data. The mission: gather every usable open aviation dataset and turn it into context in the pilot's interface, so the simulation feels complete.

Realism doesn't need huge datasets. Country and city associations, even stereotypical ones, go a long way: tulips from Amsterdam, rice from China, Japanese goods from Tokyo, electronics from manufacturing hubs.

The market is full of AI-slop MSFS products. A polished, careful product is how MyPreflight stands out.

The one deliberate exception to "no generated content" is postcards: AI-generated artwork, one per city, from a carefully crafted prompt.

## Hard lines

- **Flight simulation use only.** Never for real pilot training or operations, and clearly labelled as sim-only, the way Navigraph labels its products "for simulation use only". We sell realism and context for virtual pilots, not official knowledge. This avoids liability and false confidence.
- **No ads.** No generic or spam advertising in the app.
- **No pay-to-win.** Paid features add context and extras, never score.
- **Trust first.** Nothing that breaks trust, pushes users off-platform, or makes them feel unfairly judged.
- **As little user-generated content as possible.** Less to moderate, less to go wrong.

## Platform facts

| Fact | Value |
| --- | --- |
| Sims | MSFS 2020 and MSFS 2024 |
| Transponder | Windows only |
| Language | English |
| mypreflight.io | Static homepage, product page, documentation, sign-in links |
| app.mypreflight.io | The app |
| Licence | Open source; see `decisions.md` |
