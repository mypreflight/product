# Transponder

The Windows connector that streams flight data from the sim: what it does, how it updates, and how connection problems are shown.

Last reviewed: 2026-10-20

## Name and idea

Developed as **Transponder**. The public name isn't final.

Like a real transponder, it reports the aircraft's state. Our design is "ADS-B over HTTP": flight-tracking networks collect ADS-B from receivers; in our system the receiver is a server, so the program on the pilot's PC that transmits is the transponder. Slightly silly, deliberately so: flight geeks enjoy having their own transponder running at home.

## What it sends

The same kinds of fields a real transponder sends: position, ground speed, vertical speed, weight on wheels (airborne or not) and similar, roughly every 15 to 30 seconds.

## Footprint

- Windows only. Linux users can try Wine; no macOS or Linux builds, since they aren't worth the complexity now.
- One portable .exe. No installer, by choice.
- Console app running beside the sim, about 20 MB of RAM. Deliberately not Electron, so the sim isn't affected. This is a major selling point.
- Runs on the sim PC.
- Sign-in by email and password, or browser-based login (e.g. Google).

## Offline behaviour

No buffering, by design. If the internet drops, streaming resumes when it's back and missed positions aren't backfilled. MSFS 2024 can't be flown offline anyway, so the extra logic isn't worth it.

## Updates

- A built-in updater downloads the new version into the browser's default download folder, usually Downloads.
- It can't replace itself while running, so it tells the user a new version has been downloaded and to run it.
- Files carry the version in the name, e.g. `mypreflight-transponder-1-2-0`, so users can tell which is newer.
- Skipping an update is allowed for now. Some future releases with breaking changes may require it.
- The API is designed carefully to avoid breaking changes in the first place.

## When to start it

After the doors close and boarding is finished, before pushback. See `flight-lifecycle.md`.

## Connection problems

Today the app shows one generic message, roughly "the ADS-B server can't find the Transponder". It's a solid sign that something is off, but it covers very different causes:

| Cause | What happens |
| --- | --- |
| The browser | Windows and browsers put background tabs to sleep to save memory; Chrome and Firefox both do this, especially in energy-saver mode. The WebSocket then stops receiving updates although the server is fine. |
| A short internet drop | 10, 20 or 30 seconds, or a couple of minutes. |
| A sim crash | The sim is gone; the flight needs attention now. |

Direction:

- **Sim crash:** the Transponder detects it and sends a specific, urgent message through the API: "check your flight sim immediately". Not built yet.
- **Short drop:** a grace period, no panic, because the connection may recover.
- **Can't recover:** tell the pilot as soon as possible.
- **Open:** grace period length, somewhere between 2 and 5 minutes, decided after testing on Oskar's PC. It may depend on the phase of flight.

## Integrations

- GSX Pro API for live boarding and deboarding passenger counts (not fully wired).

## Planned

- A download page. It doesn't exist yet and is a launch blocker. It must highlight the small footprint, that the code is open source (anyone can inspect it), and how dependencies are kept up to date. Agents draft it; Oskar reviews.
