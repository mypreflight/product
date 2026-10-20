# Scoring

How points work, what is rated, and the fairness rules behind it.

Last reviewed: 2026-10-20

## Basics

- Points have no monetary value. They exist for score and leaderboard ranking.
- Groundwork exists. The points economy still needs tuning through a lot of play-testing, plus AI review and testing agents.
- Leaderboard: planned, priority not set.

## Point categories

- **Realism and continuity:** e.g. flying an airframe on from where it last landed.
- **Flight time.**
- **Distance.**
- **Special events:** themed days ("Monday is Munich Day: depart Munich once or twice for 10 points", "Germany Day") and city-pair events in the style of VATSIM events such as VATUSA's LAX–SFO. First themed event: Christmas cargo.

## Timing

Scheduled times come from SimBrief.

| Time | Rated? | Why |
| --- | --- | --- |
| Off-block | **Yes, the only rated time** | Fully in the pilot's control. Plan enough turnaround time. |
| Takeoff | Appreciated, not punished | Taxi times vary, and busy VATSIM events mean queues. |
| Landing, on-block | No | Early or late arrivals from wind and weather are realistic. |

Never push pilots to fly through weather for points.

## Rewards and penalties

| Event | Effect |
| --- | --- |
| Takeoff runway matches the selected runway | Bonus |
| Airframe departs somewhere other than where it last landed (landed Zurich, departs Frankfurt) | Repositioning fee |
| Carrying cargo | Points (amount not set, not built); penalty only if late |
| Started flight not finished | Penalty |
| Emergency | Never penalised; only handling is judged |
| Changed SID | No points either way |
| Fuel burn vs plan | Checked, not judged (weather and ATC delays aren't the pilot's fault) |

Later, only with trusted data: landing below a type-specific minimum fuel threshold, and approach accuracy (e.g. ILS alignment). Approach judging must account for changed procedures and approaches. **Never judge users on wrong data.**

## Fairness rules

- Paid features never affect score. Plain boxes score the same as premium salmon if flown equally well.
- Sponsored events are tied to an aircraft **type**, so free aircraft qualify too. Example: an A380 event is open to both the iniBuilds payware A380 and the FlyByWire freeware A380. Nobody is forced to buy.
- The Transponder API could be spoofed. The incentive is low because points have no money value; checks will be added. Manual off-blocks are compared with Transponder movement.
