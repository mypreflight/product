# Briefing content

What the briefing contains today and the rules the code follows to build it. Migrated from the backend and frontend OpenSpec specs.

Last reviewed: 2026-10-20

This file describes what is built. The decisions behind it live in `decisions.md`; where the two disagree, the decision wins and the gap is a bug or open work. The full requirement text, with scenarios, stays in the code repos (`openspec/specs/`) and their tests.

## Loadsheets

- A flight carries an ordered **history of preliminary loadsheets** (revision 1, 2, 3…) issued while the load is planned, and **at most one final loadsheet**, written once when boarding finishes.
- A loadsheet is never overwritten or deleted. Each records who issued it and when.
- A flight can't be marked ready (released to the crew) without a preliminary loadsheet, because a flight is released against the load it's planned to carry.
- Limits enforced on every loadsheet:

  | Check | Rule |
  | --- | --- |
  | Passengers | No more than the seats in the aircraft's cabin layout (skipped when no layout is assigned) |
  | Cargo | No more than the resolved hold variant can carry by weight or volume, baggage included (skipped for uncurated types) |
  | Payload | At least cargo plus passengers at the planned passenger mass |
  | Block fuel | Must equal the `block` figure of the fuel breakdown |

- The planned passenger mass comes from the flight plan itself: SimBrief configures it per airframe. A new revision inherits it.
- Weights imported from a flight plan keep kilogram precision; rounding to tenths of a tonne manufactured shortfalls of up to 100 kg.
- An optional passenger split per cabin class must add up to the total.
- The form is an operational weight build-up: dry operating → zero fuel → take-off → landing, with structural limits. A passenger count above capacity is warned about in the app before the API refuses it.

## Fuel

- The structured fuel breakdown (trip, contingency, alternate, reserve, taxi, extra, MEL/ATC/weather/ETOPS additions, tankering, planned take-off and landing fuel, average fuel flow, tank capacity) comes from the SimBrief OFP, or is typed by operations (preliminary) and the pilot (final).
- It's shown as an OFP-style ladder: taxi, trip, contingency, reserves → subtotal → additions → total, with a tank-capacity gauge. Preliminary and final are switched independently.
- On close, the pilot types the actual fuel burned, with the planned trip fuel beside it for reference.

## Passenger manifest

- Generated when the preliminary loadsheet is written, and regenerated on every later write, so it never describes a loadsheet the flight no longer has.
- Seated against the cabin layout revision **pinned** at that moment, so a later redraw of the cabin doesn't move anyone.
- Passengers are spread across cabins in proportion to each cabin's share of the seats.
- About 85% of passengers carry no special service request. The rest carry real IATA SSR codes: infant, the three wheelchair categories (ramp, steps, cabin), unaccompanied minor, blind, deaf and others. An infant occupies a seat.
- At finish-boarding the manifest is reconciled against the final loadsheet, per cabin class:
  - surplus passengers become **no-shows** who keep their seat and stay on the list;
  - a shortfall is seated in free seats;
  - everyone else is left exactly as they were.
- An aircraft with no cabin layout gets no manifest and no capacity check. The app says which kind of absence it is: not released yet, or no layout.
- Readable by operations and by the flight's captain.

## Seat maps

- Cabin layouts come from AeroLOPA. The catalogue is mirrored locally; layouts withdrawn upstream are retired, never deleted.
- Seat data is fetched on first use and stored as immutable revisions; a refresh writes a new revision only when the content changed.
- Operations assign a layout to each aircraft by hand, helped by ranked suggestions. Any airline's layout may be assigned to any aircraft; a mismatch is flagged, never refused.
- The seat drawing is schematic and says so. It's readable without a pointer, has a list equivalent, and stays legible in both themes.

## Baggage

- Derived from the loadsheet: payload minus passengers at standard adult mass minus cargo. Where that residual is negative or implausible, it falls back to the passenger count, and says which source it used.
- Converted to bags at the standard checked-baggage mass for the sector length.
- Loaded in the same hold as cargo, as containers or loose lots. Premium cabin baggage gets its own units so it comes off first.
- Kept out of the cargo tonnage.

## Cargo

The rule behind it all: cargo comes from the departure's context, never at random. Tulips from Amsterdam, salmon from Keflavik.

- **Catalogue:** 100 curated commodities, stored as data, not code. Each declares IATA handling codes, density, piece weight and count ranges, offload priority, seasons and where it comes from.
- **Selection:**
  1. Prefer commodities naming the departure airport.
  2. Then its country.
  3. Then its continent.
  4. Then the general pool.

  Seasonal commodities appear only in their months, by scheduled departure date.
- **Dangerous goods** stay under a tenth of the selection weight, and a flight carrying passengers holds at most two DG consignments.
- **Shipments** carry an 11-digit air waybill (3-digit carrier prefix, 7-digit serial, check digit = serial mod 7), a shipper and a consignee. A shipment may be one leg of a longer journey, with its onward carrier and connection time.
- **Load units** are real ULDs (IATA type code, serial, owner code, tare) placed in positions the aircraft's hold variant actually has. Bulk-loaded aircraft carry loose lots. Density decides whether a container fills by volume or by weight first.
- **Handling rules:**
  - Cargo-aircraft-only shipments are refused on a flight carrying passengers.
  - Incompatible loads are kept apart, e.g. radioactive material away from live animals, dry ice away from live animals, infectious substances away from foodstuffs.
  - Loads needing a heated or ventilated compartment go only into one.
  - A compartment holds one temperature regime unless the container controls its own.
  - The manifest reports each segregation conflict per compartment.
- **Weights:** shipments plus unit tares equal the loadsheet's cargo tonnage exactly.
- **Reconciliation at finish-boarding:** surplus shipments are offloaded in ascending offload priority (ordinary freight first; some freight is never offloaded) and kept on the manifest with a reason. Shortfalls are filled under every generation rule.
- **Hold layouts** are curated per airframe type as named variants with one default. Operations may assign a variant per aircraft; otherwise the default applies.

## Cold chain

- Every temperature-controlled shipment declares its temperature range, its solution (active container, passive shipper or dry ice) and its endurance in hours.
- Exposure is computed across the whole journey: build-up, flight, connection and onward flight. The ambient temperature at an exposed airport counts where it's known.
- The result is a risk level with a plain-language explanation of why.
- **Advisory only.** It never blocks loading, a loadsheet, release or boarding.

## Notification to captain (NOTOC)

- Issued at each loadsheet stage: when the preliminary loadsheet is written and when boarding finishes. Issued even when no dangerous goods are aboard, saying so.
- Reports every DG shipment in full (UN number, proper shipping name, class, packing group, quantities, position), the emergency drill derived from the hazard class, other notifiable loads (live animals, human remains, temperature-controlled pharma, valuables, heavy pieces), the cold-chain assessment and a load summary.
- The final notification reports what changed since the preliminary one.
- Acknowledged through actions the pilot already takes: check-in acknowledges the preliminary one, finish-boarding the final one.
- An emergency declaration fills its DG classes from the cargo manifest when the pilot gives none.
- The app shows it as a panel and as a printable document that's legible without colour.

## Weather

- Raw ATIS, METAR and TAF per airport and per source: aviationweather.gov (METAR, TAF) and SayIntentions (ATIS, METAR, TAF). Each pilot picks a default source.
- An airport is monitored from check-in until on-block of any flight using it, and refreshed on a schedule. A failure at one source doesn't affect the other.
- The weather endpoint is public and returns an empty list rather than an error when nothing is stored.

## NOTAMs

- Taken from the imported SimBrief OFP. Each import replaces the stored NOTAMs of the airports in that plan only.
- Served publicly per airport, filtered to those in force.

## Crew

- The first officer, purser and cabin crew come from the SimBrief roster as operator-owned background characters. They never sign in. The import is idempotent.
- Operations can also assign crew from the operator's pool.

## Discord briefing

At check-in the captain gets a Discord DM with:

- the flight, route and aircraft;
- the estimated out/off/on/in schedule and block time;
- departure ATIS, METAR and TAF exactly as published;
- a special load summary pointing to the NOTOC;
- a link back to the flight;
- the OFP attached when the flight came from SimBrief.

See `platform-rules.md` for the other messages.
