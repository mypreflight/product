# Transponder

The Windows connector that streams flight data from the sim: what it does, how it updates, and how connection problems are shown.

Last reviewed: 2026-11-03

## Name and idea

Developed as **Transponder**. The public name isn't final.

Like a real transponder, it reports the aircraft's state. Our design is "ADS-B over HTTP": flight-tracking networks collect ADS-B from receivers; in our system the receiver is a server, so the program on the pilot's PC that transmits is the transponder. Slightly silly, deliberately so: flight geeks enjoy having their own transponder running at home.

## What it sends

The same kinds of fields a real transponder sends: position, ground speed, vertical speed, weight on wheels (airborne or not) and similar, roughly every 15 to 30 seconds.

## Footprint

- Windows only. Linux users can try Wine; no macOS or Linux builds, since they aren't worth the complexity now.
- One portable .exe. No installer, by choice.
- Console app running beside the sim, about 20 MB of RAM. Deliberately lightweight, so the sim isn't affected. This is a major selling point.
- Runs on the sim PC.
- Everything it's built from is checked every day for known security issues, and an update to any of it is reviewed before it ships. Nothing it relies on can change without that review.
- Sign-in through the browser only. See below.

## Sign-in

- The Transponder never asks for a password. It shows a short code and opens MyPreflight in the browser, where the pilot signs in the usual way (password, Google or Discord), checks that the code matches and approves. A pilot who didn't start the sign-in declines it instead.
- The code can be typed on any device, so the browser doesn't have to run on the sim PC.
- A code lasts 10 minutes and works once.
- A Transponder sign-in lasts 90 days from its last use, so a pilot who flies regularly approves it once.
- The Transponder can do only what it needs: read the pilot, their flight, the Discord presence and the GSX switches, and report boarding and deboarding. Everything else is refused, from account settings to approving another device. Only the Transponder can report a step as coming from GSX.
- Pilots see each signed-in Transponder, named after its computer, on their account page and can sign it out there.

## Offline behaviour

No buffering, by design. If the internet drops, streaming resumes when it's back and missed positions aren't backfilled. MSFS 2024 can't be flown offline anyway, so the extra logic isn't worth it.

## Updates

- A built-in updater downloads the new version into the browser's default download folder, usually Downloads.
- It can't replace itself while running, so it tells the user a new version has been downloaded and to run it.
- Files carry the version in the name, so users can tell which is newer.
- Skipping an update is allowed for now. Some future releases with breaking changes may require it.
- We work carefully to avoid breaking changes in the first place.

## When to start it

After the doors close and boarding is finished, before pushback. See `flight-lifecycle.md`.

## Connection problems

Today the app shows one generic message, roughly "the ADS-B server can't find the Transponder". It's a solid sign that something is off, but it covers very different causes:

| Cause | What happens |
| --- | --- |
| The browser | Windows and browsers put background tabs to sleep to save memory; Chrome and Firefox both do this, especially in energy-saver mode. The page then stops receiving updates although the service is fine. |
| A short internet drop | 10, 20 or 30 seconds, or a couple of minutes. |
| A sim crash | The sim is gone; the flight needs attention now. |

Direction:

- **Sim crash:** the Transponder detects it and sends a specific, urgent message: "check your flight sim immediately". Not built yet.
- **Short drop:** a grace period, no panic, because the connection may recover.
- **Can't recover:** tell the pilot as soon as possible.
- **Open:** grace period length, somewhere between 2 and 5 minutes, decided after testing on Oskar's PC. It may depend on the phase of flight.

## Integrations

- GSX Pro API for live boarding and deboarding passenger counts (not fully wired).

### GSX switches

Each pilot has three GSX switches in their settings:

| Switch | Starts as | What it does |
| --- | --- | --- |
| GSX integration | Off | Master switch for the automatic steps. The Transponder reads GSX and shows the turnaround either way. |
| Automatic boarding | On | Boarding starts by itself when the pilot requests it through GSX. |
| Automatic deboarding | On | Deboarding starts by itself when the pilot requests it through GSX, and finishes by itself when GSX completes it. |

The two automatic switches apply only while the integration is on. Turning the integration off keeps their positions, so turning it back on restores the pilot's choices.

With the switches on, the Transponder starts boarding when the pilot requests it through GSX on a checked-in flight, and starts offboarding when they request deboarding on a flight that's on block. If deboarding is requested before on-block is reported, the Transponder waits, and starts offboarding once on-block is reported if GSX is still running it. When GSX completes deboarding, the Transponder finishes offboarding, however it was started. A bypassed deboarding doesn't count, because passengers weren't seen off. Finishing boarding stays with the pilot, because it needs the final loadsheet, and nothing is ever sent to GSX.

Pilots change the switches on their account page. The integration starts off, so nobody gets the automatic steps without asking for them. Only the flying role sees these switches.

- **Open:** whether the integration starts on or off for a new pilot. It starts off for now, because not every pilot flies with GSX.

## Planned

- A download page. It doesn't exist yet and is a launch blocker. It must highlight the small footprint, that it's open source (anyone can inspect it), and how it's kept up to date. Agents draft it; Oskar reviews.
