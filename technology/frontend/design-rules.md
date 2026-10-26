# Design rules

Rules that make dozens of views, modals and components look like one app.

Last reviewed: 2026-10-20

Sources: the brief, the frontend repo's `DESIGN.md` and `PRODUCT.md`, the Flowbite theme (`app/styles/theme.ts`) and the frontend OpenSpec UI specs. Colours, fonts and the logo live in `../../product/brand/identity.md`.

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

- Use components and variants from the Flowbite theme file; don't style one-offs. Overrides go in `app/styles/theme.ts` (`createTheme`), never in per-call `className` hacks.
- Themed Flowbite components are used directly; there are no wrapper components.
- Form fields use the managed blocks in `app/shared/ui/Form/Managed/` (Formik-aware), not raw Flowbite inputs.

| Button `color` | Use |
| --- | --- |
| `indigo` | The one primary action on a view or tile |
| `alternative` | Secondary |
| `subtle` | Low-emphasis links ("History", "View all") |
| `light` | Neutral chip-like action |
| `warning` | A step the pilot must do by hand because automation can't, e.g. reporting takeoff while automatic detection is unavailable |
| `red` | Destructive only |

| Badge `color` | Meaning |
| --- | --- |
| `success` | Finished |
| `warning` | Pending, in transit |
| `info` | Dead-head |
| `gray` | Automatic, neutral |
| `indigo` | Performing flight |

Badges are pills: uppercase, bold, 11–12 px, a tint background with saturated text, always with a word.

## Colour

- **The Rare Accent Rule.** Indigo appears on at most 10% of a screen: actions and the one current selection. If two things on a panel are indigo, one is wrong.
- **Signal, not decoration.** Colour always means state, status or action. Gray does the structural work.
- **Never colour-only.** Every status colour is paired with a word or icon; warnings and errors use bold text, icons or modals. A red border with no label is not a message.
- **Integration brand exception.** A third-party integration may use its own hue on its own marks only (glyph, avatar ring), never on actions, text or surfaces. Discord is the only one today.

## Typography

- **Tabular Figures Rule.** Any number a user compares or sums is set in the mono font with `tabular-nums`, right-aligned in columns, unit suffix smaller and muted.
- **Micro-Caps Rule.** Uppercase tracked labels (at least 0.06em) for field labels and column keys; never for reading-length text.
- **Wordmark Rule.** In prose the name is **MyPreflight**. As a logotype it is **mypreflight**, lowercase, `my` regular and `preflight` bold, rendered only through the `Wordmark` component.
- A fixed rem scale, never fluid:

  | Role | Weight, size |
  | --- | --- |
  | Display (flight and page titles) | 700, 1.875rem |
  | Headline (panel sections) | 700, 1.25rem |
  | Title (cards, groups) | 600, 1rem |
  | Body | 400, 0.875rem; prose capped at 65–75ch |
  | Label | 700, 0.6875rem, uppercase, 0.08em tracking |
  | Data | Mono 500, 0.875rem, tabular |

## Surfaces and spacing

- **Flat Surface Rule.** Panels are separated by a 1 px border and a one-step tonal change, never a drop shadow. The only sanctioned shadow is a modal lifting off its backdrop. The faint card shadow in the theme is legacy.
- Radii: cards 16 px (`rounded-2xl`); modals and inner data panels 12 px (`rounded-xl`); buttons and inputs 8 px (`rounded-lg`); pills full.
- Padding: cards 20 px, inner panels 16 px; container padding `condensed` 16, `normal` 20, `spacious` 24; 16 px gap between blocks.
- Empty state: a quiet dashed placeholder on `gray-50`, with a hint that says what will appear and when (e.g. "Close your first flight and your statistics will appear here.").

## Interaction and accessibility

- WCAG 2.1 AA in both themes. Body text at 4.5:1; never `gray-400` or lighter for body text on near-white tints.
- Every control ships default, hover, focus, active and disabled states. Focus is a visible indigo ring.
- Motion only conveys state: 150–250 ms transitions, and `prefers-reduced-motion` is honoured.
- Inputs use floating labels. Errors appear as a message below the field, never as a red border alone.
- **Confirm before a step that can't be undone.** The dialog names what happens next ("AA4912 moves to Taxiing in"), its confirm button repeats the action by name rather than "OK", focus doesn't start on the confirm button, and a failed request keeps the dialog open with the error. A step that already opens its own form gets no second confirmation.
- **A blocked action says why, next to it.** Keep the button visible but disabled, and put the reason and a link to the fix beside it. Never hide the only explanation in a tooltip on a disabled button.
- Tabs use the underline variant; an overflowing tablist scrolls horizontally rather than wrapping.
- Light and dark themes both ship and follow the operating system automatically.

## Layout

- Phone and tablet layouts are checked for every view; the app is a second screen beside the sim.
- Lists work from a 390 px viewport without horizontal scrolling. Secondary lines hide below `sm`; nothing else does.
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

- Draw from the data's own geometry, one deck at a time, with clear deck switching. Never request a provider's image assets.
- Seat maps are schematic and say so; spacing between cabins may be compressed. Holds are drawn nose to tail, with the deck width the aircraft really has.
- Seats and positions never shrink below a usable hit size; the drawing scrolls horizontally instead.
- Meaning is never colour alone: heating, ventilation and door side carry text or a marker.
- Every drawing has a table equivalent, available to every reader, not only to assistive technology.
- Absence is stated plainly and by kind ("not catalogued" vs "provider unreachable", "type not curated" vs "request failed"), never shown as an error or an empty drawing.

## Maps (Leaflet)

- **Colour roles:** indigo for the tracked flight's own elements (planned route, endpoint labels, airport boundary, assigned runway, aircraft); orange for terminals and gates; green for parking stands; red for diversion endpoints. No other hues.
- **Airport labels:** indigo text on a crisp contrast halo (dark halo on dark tiles, light on light), no card, no anchor dot. IATA only until terminals and runways appear.
- **Progressive detail by zoom:** boundary first, then terminals and runways, then gates and stands.
- **Markers:** gates as orange chips with a mono designator; stands as green dots, filled for the assigned stand and hollow for the rest. Never teardrop pins.
- **Runways:** a real-scale asphalt ribbon built from the runway's real width.
- **Routes:** the planned great-circle line is thin, dashed and beneath the altitude-coloured flown track.
- **Order:** the aircraft marker always sits above every label and marker.
- **Telemetry:** altitude, ground speed, track and vertical rate as compact small caps centred in the map top bar; hidden when there's no room.
- **Tiles:** CARTO raster basemaps; vector tiles cap at zoom 14, too low for aprons.
