# Briefing content

What the briefing contains today and the rules it follows.

Last reviewed: 2026-11-16

This file describes what is built. The decisions behind it live in `decisions.md`; where the two disagree, the decision wins and the gap is a bug or open work.

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
  | Block fuel | Must equal the block figure of the fuel breakdown |

- The planned passenger mass comes from the flight plan itself: SimBrief configures it per airframe. A new revision inherits it.
- Weights imported from a flight plan keep kilogram precision; rounding to tenths of a tonne manufactured shortfalls of up to 100 kg.
- An imported plan with more passengers than the cabin layout has seats is capped at the seat count. The mass of the passengers left behind, at the planned passenger mass, comes off the payload and the zero fuel weight; cargo and fuel stay as planned. A loadsheet typed by operations is refused instead.
- An optional passenger split per cabin class must add up to the total.
- The form is an operational weight build-up: dry operating → zero fuel → take-off → landing, with structural limits. A passenger count above capacity gets a warning in the app before it's refused.

## Fuel

- The structured fuel breakdown (trip, contingency, alternate, reserve, taxi, extra, MEL/ATC/weather/ETOPS additions, tankering, planned take-off and landing fuel, average fuel flow, tank capacity) comes from the SimBrief OFP, or is typed by operations (preliminary) and the pilot (final).
- It's shown as an OFP-style ladder: taxi, trip, contingency, reserves → subtotal → additions → total, with a tank-capacity gauge. Preliminary and final are switched independently.
- On close, the pilot types the actual fuel burned, with the planned trip fuel beside it for reference.

## Passenger manifest

- Generated when the preliminary loadsheet is written, and regenerated on every later write, so it never describes a loadsheet the flight no longer has.
- Seated against the cabin layout revision **pinned** at that moment, so a later redraw of the cabin doesn't move anyone.
- Passengers are spread across cabins in proportion to each cabin's share of the seats.
- About 85% of passengers carry no special service request. The rest carry real IATA SSR codes: infant, the three wheelchair categories (ramp, steps, cabin), unaccompanied minor, blind, deaf and others. An infant occupies a seat.
- The passenger information list (PIL) shows only the passengers with a special service request, filterable by code, each code spelled out.
- At finish-boarding the manifest is reconciled against the final loadsheet, per cabin class:
  - surplus passengers become **no-shows** who keep their seat and stay on the list;
  - a shortfall is seated in free seats;
  - everyone else is left exactly as they were.
- An aircraft with no cabin layout gets no manifest and no capacity check. The app says which kind of absence it is: not released yet, or no layout.
- Readable by operations and by the flight's captain.

## Seat maps

- Cabin layouts come from AeroLOPA. The catalogue is mirrored locally; layouts withdrawn upstream are retired, never deleted.
- Seat data is fetched on first use and kept as revisions that never change; a refresh writes a new revision only when the content changed.
- Operations assign a layout to each aircraft by hand, helped by ranked suggestions. Any airline's layout may be assigned to any aircraft; a mismatch is flagged, never refused.
- The seat drawing is schematic and says so. It's readable without a pointer, has a list equivalent, and stays legible in both themes.

## Baggage

- Derived from the loadsheet: payload minus passengers at standard adult mass minus cargo. Where that residual is negative or implausible, it falls back to the passenger count, and says which source it used.
- Converted to bags at the standard checked-baggage mass for the sector length.
- Loaded in the same hold as cargo, as containers or loose lots. Premium cabin baggage gets its own units so it comes off first.
- Kept out of the cargo tonnage.

## Cargo

The rule behind it all: cargo comes from the departure's context, never at random. Tulips from Amsterdam, salmon from Keflavik.

- **Catalogue:** 100 curated commodities. Each declares IATA handling codes, density, piece weight and count ranges, offload priority, seasons and where it comes from.
- **Selection:**
  1. Prefer commodities naming the departure airport.
  2. Then its country.
  3. Then its continent.
  4. Then the general pool.

  Seasonal commodities appear only in their months, by scheduled departure date. A commodity the destination's continent doesn't want is drawn a quarter as often, so flowers fly into Europe more than into Oceania.
- **Dangerous goods** stay under a tenth of the selection weight, and a flight carrying passengers holds at most two DG consignments.
- **Shipments** carry an 11-digit air waybill (3-digit carrier prefix, 7-digit serial, check digit = serial mod 7), a shipper and a consignee. A shipment may be one leg of a longer journey, with its onward carrier and connection time.
- **Load units** are real ULDs (IATA type code, serial, owner code, tare) placed in positions the aircraft's hold variant actually has. Bulk-loaded aircraft carry loose lots. Density decides whether a container fills by volume or by weight first.
- **Handling rules:**
  - Cargo-aircraft-only shipments are refused on a flight carrying passengers.
  - Incompatible loads are kept apart, e.g. radioactive material away from live animals, dry ice away from live animals, infectious substances away from foodstuffs.
  - Loads needing a heated or ventilated compartment go only into one.
  - A heavy or outsized piece goes only into a device it fits, floor and height, without being tipped on its side: a coffin on a pallet or loose in the bulk hold, never in an LD3; horse stalls only in a main-deck container. Loose pieces must pass the bulk door.
  - A compartment holds one temperature regime unless the container controls its own.
  - The manifest reports each segregation conflict per compartment. The cargo page's load checks list each one with both loads in words and the waybills carrying them.
- **Weights:** shipments plus unit tares equal the loadsheet's cargo tonnage exactly.
- **Reconciliation at finish-boarding:** surplus shipments are offloaded in ascending offload priority (ordinary freight first; some freight is never offloaded) and kept on the manifest with a reason. Shortfalls are filled under every generation rule.
- **Hold layouts** are curated per airframe type as named variants with one default. Operations may assign a variant per aircraft; otherwise the default applies.

## Cold chain

- Every temperature-controlled shipment declares its temperature range, its solution (active container, passive shipper or dry ice) and its endurance in hours. Frozen loads packed in dry ice carry that dry ice, and it counts towards the compartment's dry ice.
- Exposure is computed across the whole journey: build-up, flight, connection and onward flight. The ambient temperature at an exposed airport counts where it's known.
- The result is a risk level with a plain-language explanation of why.
- **Advisory only.** It never blocks loading, a loadsheet, release or boarding.
- Shown on the loadsheet, beside the load in each compartment, for the stage the loadsheet switch shows. Not on the NOTOC, which carries only what the paper form carries.

## Notification to captain (NOTOC)

- Issued at each loadsheet stage: when the preliminary loadsheet is written and when boarding finishes. Issued even when no dangerous goods are aboard, saying so.
- Carries what the IATA-style paper form carries, and nothing else:
  - **Header:** operator, flight number and date, port of loading, aircraft registration and type, who prepared it and who supervised loading (names from the departure station, the same on every issue of a flight), and an emergency telephone number from a range reserved for fiction.
  - **Dangerous goods**, one row each: air waybill, proper shipping name, class or division (with the compatibility group for explosives), UN number, subsidiary risk, packages, net quantity, packing group, handling code, cargo-aircraft-only, ULD and position, unloading airport and the emergency response code, which opens the drill derived from the hazard class. Radioactive material shows its package category and transport index instead of a net quantity. Dry ice is one row per compartment with its total. Category B biological samples and lithium batteries in small equipment are left off, as the regulations allow.
  - **Special load:** live animals, human remains, perishables and temperature-controlled loads, pharma, valuables, heavy and outsized pieces, and the rest that must be notified, each with packages, weight, ULD and position and supplementary information: handling remarks, the temperature to keep, the container or packing, the hold conditions and the heaviest piece (never heavier than the shipment's own pieces).
  - **Special category passengers:** seat, code and remarks, including where a battery-powered wheelchair and its battery travel.
  - **Certification:** the loading supervisor's and the captain's acceptance.
- The final notification reports what changed since the preliminary one, including each load that moved, from the position it left to the one it holds now.
- Positions are written the way the hold layout writes them: compartment, row and side on the lower deck (12R), a letter and side on the main deck (CL), and P for a position spanning the width. The same position reads the same on the hold diagram, the cargo manifest and the NOTOC.
- Notifications issued on the earlier form, before November 2026, are no longer shown: the flight reads as having none until a new one is issued.
- Acknowledged through actions the pilot already takes: check-in acknowledges the preliminary one, finish-boarding the final one.
- An emergency declaration fills its DG classes from the cargo manifest when the pilot gives none.
- The app shows it digitally. The dangerous goods, special load and special category passengers sections each show how many entries they hold and fold to a one-line summary: dangerous goods by name, special loads by handling code, passengers by service request with their seat count. A NOTOC with no dangerous goods says plainly that none are loaded.
- It downloads as a PDF laid out column for column like the paper form: landscape A4, black on white, with a 10 mm margin on every side so it prints whole. A long NOTOC continues onto further sheets, each naming the notification, flight, date, registration and port of loading, and a table repeats its heading there. Its instructions and certifications name the flight's operator where a paper form names its owner, and the top left of its first page carries the MyPreflight logo in dark grey where a paper form carries its owner's mark.

## Weather

- Raw ATIS, METAR and TAF per airport and per source: aviationweather.gov (METAR, TAF) and SayIntentions (ATIS, METAR, TAF). Each pilot picks one of these two as their default source.
- Three more sources give ATIS only: the FAA's real digital ATIS for US airports, real digital ATIS that aircraft requested over datalink worldwide (atis.guru), and VATSIM controllers' ATIS. An airport with a separate arrival and departure ATIS keeps both, and each ATIS carries the time its source published it. An ATIS a source stops publishing disappears, such as when a VATSIM controller logs off.
- Each ATIS, from any source, carries the runways in use for arrivals and for departures, always runways the airport has. A runway the ATIS names only as closed, out of service, for holding short, for the wind or in its NOTAMs doesn't count. A runway put in use without saying for what counts for both on a combined ATIS, and for its own side on a separate arrival or departure ATIS. METAR and TAF carry no runways.
- Pilots pick the ATIS from SayIntentions or Real. Real is the newer of the FAA's and atis.guru's ATIS for that airport and end, so it isn't a third choice of its own. Each pilot sets a default in Settings, next to the weather source, and can switch in the briefing and the runway picker. VATSIM ATIS is collected but not offered.
- The ATIS information letter is read from the ATIS text, such as "information Kilo" or "INFO K".
- METAR and TAF are kept without the METAR or TAF word in front, which only repeats what the report is. A SPECI, and an amendment or correction marker, stay.
- An airport is monitored from check-in until on-block of any flight using it, and refreshed on a schedule. A failure at one source doesn't affect the others.
- Weather is public. An airport with nothing stored shows an empty list, not an error.

## Airport, stand and runway

- Preflight opens with the departure airport and Arrival with the arrival airport: its weather on one side, the stand and runway on the other. The weather is the METAR and TAF from the source the pilot picks and the ATIS from the ATIS source they pick, for that end: the departure ATIS at departure, the arrival ATIS at arrival, or the airport's single ATIS.
- The stand shows its terminal and the airlines that use it, whether it's at a gate or remote, how passengers board, the parking guidance, the services, de-icing, noise sensitivity with its hours, and its position in degrees and decimal minutes, for aligning the IRS.
- The runway shows its surface and lighting, magnetic heading, length with any displaced threshold, width, and elevation in feet.
- Chips sit next to the runway. The ATIS chip carries the information letter: green with ARR, DEP or both when the ATIS has the runway in use for this end, amber naming the runways it uses instead when it doesn't (such as DEP 09). It's left out when the ATIS names no runway for this end.
- The wind chip gives the METAR wind from the pilot's weather source, worked out against the runway's true heading, in knots: along the runway, an arrow down for a headwind and up for a tailwind, which turns the chip amber; and across it, an arrow showing the side it blows from. A calm or variable wind says so.
- The runway picker lists every runway as its designator, its chips, then its length, width, surface and course, under the ATIS source, the provider and time of the ATIS, and the METAR wind. Operations get the same picker on the flight page.

## NOTAMs

- Taken from the imported SimBrief OFP. Each import replaces the stored NOTAMs of the airports in that plan only.
- Served publicly per airport, filtered to those in force.

## Crew

- The first officer, purser and cabin crew come from the SimBrief roster as operator-owned background characters. They never sign in. Importing a roster again creates no duplicates.
- Operations can also assign crew from the operator's pool.

## OFP and runway analysis

- Both are SimBrief's own text, kept as SimBrief laid it out, column for column, and sized to fill the briefing panel's width. Only a line wider than SimBrief's usual width scrolls sideways.
- Each opens with its name, Operational flight plan or Takeoff and landing report, and the release or report number and the time SimBrief issued it, where the text states them.
- Download PDF saves SimBrief's own PDF of the plan, under SimBrief's file name, not a copy the app makes. SimBrief keeps it only for a while, and the app says so when it's gone. The runway analysis has no PDF of its own: SimBrief's OFP PDF carries it, so it offers the same one.

## Discord briefing

At check-in the captain gets a Discord DM with:

- the flight, route and aircraft;
- the estimated out/off/on/in schedule and block time;
- departure ATIS, METAR and TAF exactly as published;
- a special load summary pointing to the NOTOC;
- a link back to the flight;
- the OFP attached when the flight came from SimBrief.

See `platform-rules.md` for the other messages.
