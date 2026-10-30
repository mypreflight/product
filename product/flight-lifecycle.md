# Flight lifecycle

What happens from SimBrief import to a finished flight, step by step.

Last reviewed: 2026-10-20

| # | Step | How it works |
| --- | --- | --- |
| 1 | Import | The pilot imports a SimBrief flight. The briefing is generated: manifest, cargo, loadsheet, seat map, weather, NOTAMs, gate, stand, runway. |
| 2 | Boarding | The Transponder is not expected to run yet. Real aircraft have it off while passengers board. |
| 3 | Doors closed | The pilot starts the Transponder once the doors are closed and boarding is finished, before pushback. This mirrors the real Mode S procedure, and the product teaches users how it works. |
| 4 | Off-block | Detected automatically from the first position report with ground speed above **3 knots**; below that the aircraft counts as standing. See "Why 3 knots" below. The pilot can also report off-block by hand and confirm it in a dialog; the time recorded is the moment they confirm. There's no typed time. |
| 5 | Taxi and takeoff | The actual takeoff runway is compared with the selected one (bonus if they match). Takeoff punctuality is appreciated, never punished. |
| 6 | Cruise | Position reports roughly every 15 to 30 seconds. |
| 7 | Live view | Planned for launch, partly built. Built: the flight page's Tracking view, a full-page live map from off-block on. See "Live view" below. |
| 8 | Emergency or diversion | Pilot-declared. A diversion ends the flight at the diversion airport. |
| 9 | Landing | Detected automatically. The target is zero missed landings. |
| 10 | On-block | The pilot reports it (chocks on) and confirms it in a dialog. It isn't reliably detectable, and a real tablet wouldn't know either. |
| 11 | Disembark | The pilot is prompted to disembark. Live passenger counts come from the GSX Pro API through the Transponder, like crew counting passengers off (not fully wired yet). |
| 12 | Finish | The pilot enters the actual fuel burn by hand. This is deliberate realism: a real EFB has no link to aircraft systems, even though the sim could provide the number. |
| 13 | Not finished | Unfinished flights are penalised (laziness, skipping the post-flight steps). Planned: auto-finish about 24 hours after the scheduled time, so stale flights don't show as live. |

## Why 3 knots

Reported GPS positions are never exactly the same, as in the real world, so a parked aircraft jitters slightly; the threshold absorbs that. Average pushback speed is about 8 knots (from Oskar's research and conversations with airport people).

## Live view

The full flight plan is already shown well. Coming:

- Planned vs actual fuel at the next waypoint, e.g. at the oceanic boundary between Shanwick and New York Oceanic, with the pilot entering current fuel.
- Trend monitoring raises a high-priority warning when figures look dangerous (a possible fuel leak). The pilot in command always decides.
- It needs waypoint coordinates; open-source nav data such as the VATSIM map uses may help.

## When the manual off-block button is used

- The Transponder isn't working. The pilot can still track progress and sort out the Transponder later.
- The Transponder was started at the same moment as pushback, to speed things up.

The button records a time, nothing more. Someone could still fake it, so manual off-blocks are compared with the movement the Transponder later reports.
