# Design rules

Rules that make dozens of views, modals and components look like one app.

Last reviewed: 2026-10-20

Colours, fonts and the logo live in `identity.md`.

## Goal for launch

One consistent, capable-looking app. The biggest pre-launch gap is inconsistency, not missing features.

## North star: "The Flight Plan"

The interface reads like the paperwork of a real operation: a flight plan, a loadsheet, a fuel log. Exact figures, labelled units, visible subtotals, nothing decorative between the reader and the number. The document *is* the interface.

- **Trust the numbers.** Weights, fuel and times are the product. Show them exactly, with units and with the derivation visible (build-ups, subtotals, running totals).
- **Earned familiarity over novelty.** Tabs, side nav, tables; modals only when nothing inline works.
- **Density with legibility.** Hierarchy and rhythm carry density, not whitespace alone.
- **Role-appropriate surfaces.** Each role sees only what it acts on.
- **Procedural realism, not theatre.** Realism comes from correct workflow and terminology.
- **Minimal but rich.** Every screen should feel thought-through, never empty.

Rejected: the flashy gamer or sci-fi HUD (neon, glows, aggressive dark mode, fake-cockpit skeuomorphism), and playful consumer-SaaS softness.

## Components

Use the app's defined button and badge styles. No one-offs.

| Button | Use |
| --- | --- |
| Indigo | The one primary action on a view or tile |
| Alternative | Secondary |
| Subtle | Low-emphasis links ("History", "View all") |
| Light | Neutral chip-like action |
| Warning | A step the pilot must do by hand because automation can't, e.g. reporting takeoff while automatic detection is unavailable |
| Red | Destructive only |

| Badge | Meaning |
| --- | --- |
| Success | Finished |
| Warning | Pending, in transit |
| Info | Dead-head |
| Gray | Automatic, neutral |
| Indigo | Performing flight |

Badges are pills: uppercase, bold, 11–12 px, a tint background with saturated text, always with a word.

## Colour

- **The Rare Accent Rule.** Indigo appears on at most 10% of a screen: actions and the one current selection. If two things on a panel are indigo, one is wrong.
- **Signal, not decoration.** Colour always means state, status or action. Gray does the structural work.
- **Never colour-only.** Every status colour is paired with a word or icon; warnings and errors use bold text, icons or modals. A red border with no label is not a message.
- **Integration brand exception.** A third-party integration may use its own hue on its own marks only (glyph, avatar ring), never on actions, text or surfaces. Discord is the only one today.

## Typography

- **Tabular Figures Rule.** Any number a user compares or sums is set in the mono font with tabular figures, right-aligned in columns, unit suffix smaller and muted.
- **Micro-Caps Rule.** Uppercase tracked labels (at least 0.06em) for field labels and column keys; never for reading-length text.
- **Wordmark Rule.** In prose the name is **MyPreflight**. As a logotype it is **mypreflight**, lowercase, `my` regular and `preflight` bold.
- A fixed type scale, never fluid:

  | Role | Weight, size |
  | --- | --- |
  | Display (flight and page titles) | 700, 30 px |
  | Headline (panel sections) | 700, 20 px |
  | Title (cards, groups) | 600, 16 px |
  | Body | 400, 14 px; prose capped at 65–75 characters a line |
  | Label | 700, 11 px, uppercase, 0.08em tracking |
  | Data | Mono 500, 14 px, tabular |

## Surfaces and spacing

- **Flat Surface Rule.** Panels are separated by a 1 px border and a one-step tonal change, never a drop shadow. The only sanctioned shadow is a modal lifting off its backdrop. The faint card shadow still in the app is legacy.
- Radii: cards 16 px; modals and inner data panels 12 px; buttons and inputs 8 px; pills full.
- Padding: cards 20 px, inner panels 16 px; containers 16, 20 or 24 px by density; 16 px gap between blocks.
- Empty state: a quiet dashed placeholder on Gray 50, with a hint that says what will appear and when (e.g. "Close your first flight and your statistics will appear here.").

## Interaction and accessibility

- WCAG 2.1 AA in both themes. Body text at 4.5:1; never Gray 400 or lighter for body text on near-white tints.
- Every control ships default, hover, focus, active and disabled states. Focus is a visible indigo ring.
- Motion only conveys state: 150–250 ms transitions, and the system's reduced-motion setting is honoured.
- Inputs use floating labels. Errors appear as a message below the field, never as a red border alone.
- **Confirm before a step that can't be undone.** The dialog names what happens next ("AA4912 moves to Taxiing in"), its confirm button repeats the action by name rather than "OK", focus doesn't start on the confirm button, and a failed request keeps the dialog open with the error. A step that already opens its own form gets no second confirmation.
- **Icon-only buttons say what they do.** A visible tooltip on hover and focus, not the browser's delayed hint; a copy action confirms in the same tooltip ("Tracking link copied"). On touch screens every icon button is 44 px, including a link styled as a button.
- **Views switch in place.** Moving between Briefing and Tracking cross-fades what lies beneath the flight cards while the cards hold still and the switch slides across; the map stays loaded between visits so it never redraws from blank.
- **A blocked action says why, next to it.** Keep the button visible but disabled, and put the reason and a link to the fix beside it. Never hide the only explanation in a tooltip on a disabled button.
- Tabs use the underline style; an overflowing row of tabs scrolls horizontally rather than wrapping.
- Light and dark themes both ship and follow the operating system automatically.

## Layout

- Phone and tablet layouts are checked for every view; the app is a second screen beside the sim.
- Lists work from a 390 px viewport without horizontal scrolling. Secondary lines hide on small screens; nothing else does.
- A whole row is a real link (new tab, copy address and modified clicks all work), with in-row controls as the exception.
- Lists are exposed as lists, and each row has one accessible name (e.g. callsign, route, status), not four unlabelled fragments.

## Signature patterns

- **Fuel build-up.** A vertical ladder of mono figures: taxi, trip, contingency, reserves → subtotal → additions → total, with ruled subtotals, a running total and a slim tank-capacity gauge. It's the good example of high-value information shown well: fuel in tonnes and minutes.
- **Flight list row.** Four columns, at most two lines each:

  | Column | Primary line | Secondary line |
  | --- | --- | --- |
  | Date | ICAO-style off-block date (`19AUG`; the year only when it isn't this year) | Off-block time in UTC (`06:20Z`) |
  | Flight | Callsign, the visual anchor | Registration |
  | Route | `FRA → MUC` | Airport names |
  | Trailing | Block time (finished) or status (upcoming, live) | Air time (`air 1:32`) |

- **Loadsheet form.** An operational weight build-up (dry operating → zero fuel → take-off → landing) with limit-aware validation, shared by the preliminary and final flows. The final flow opens with what changed since the preliminary loadsheet, as before → after with the difference ("Passengers 296 → 294 −2"), updating as the pilot types.
- **Lifecycle track.** The flight page shows every stage from check-in to close on one track, each with its time. The current stage takes the most width and fills as it progresses.
- **Next-step panel.** The next action on the flight page is a question with one button ("Is boarding complete?", "Airborne?"). While automation is watching, the panel says what for, and the manual button steps down to an outline.
- **Deviation in words.** A time compared with its schedule reads "+20 min late", "5 min early" or "On time", never a signed number alone.
- **Documents.** The NOTOC also renders as a printable document, legible without colour, carrying the standard hazard marks.
- **Bad example to fix:** cargo types and cold-chain details are currently shown poorly. High-value information gets prominent, clear treatment.

## Drawings: seat maps and cargo holds

- Draw from the data's own geometry, one deck at a time, with clear deck switching. Never use a provider's images.
- Seat maps are schematic and say so; spacing between cabins may be compressed. Holds are drawn nose to tail, with the deck width the aircraft really has.
- Seats and positions never shrink below a usable hit size; the drawing scrolls horizontally instead.
- Meaning is never colour alone: heating, ventilation and door side carry text or a marker.
- Every drawing has a table equivalent, available to every reader, not only to assistive technology.
- Absence is stated plainly and by kind ("not catalogued" vs "provider unreachable", "type not curated" vs "request failed"), never shown as an error or an empty drawing.

## Maps

- **Colour roles:** indigo for the tracked flight's own elements (planned route, endpoint labels, airport boundary, assigned runway, aircraft); orange for terminals and gates; green for parking stands; red for diversion endpoints. No other hues.
- **Airport labels:** indigo text on a crisp contrast halo (dark halo on dark tiles, light on light), no card, no anchor dot. IATA only until terminals and runways appear.
- **Progressive detail by zoom:** boundary first, then terminals and runways, then gates and stands.
- **Markers:** gates as orange chips with a mono designator; stands as green dots, filled for the assigned stand and hollow for the rest. Never teardrop pins.
- **Runways:** a real-scale asphalt ribbon built from the runway's real width.
- **Routes:** the planned great-circle line is thin, dashed and beneath the altitude-coloured flown track.
- **Order:** the aircraft marker always sits above every label and marker.
- **Telemetry:** altitude, ground speed, track and vertical rate as compact small caps; in the map's bottom bar on the Tracking view, centred in the map top bar elsewhere, hidden when there's no room.
- **Maps under cards:** a map that runs behind floating cards centres within the part left visible below them. In cruise the Tracking view fits the whole filed route and the aircraft into that space.
- **Recentring:** corrections from data arriving or layout settling are instant; the map glides only when what it follows changes (a new phase, a different follow choice), and after the pilot pans or zooms it waits 7.5 s before returning.
- **Tiles:** CARTO basemaps, because they keep the detail aprons, stands and gates need. The free alternatives we evaluated stop too early.
