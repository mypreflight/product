# MyPreflight knowledge base

The operating manual for MyPreflight: product decisions, business and brand. Start here, then read only the files your task needs.

Last reviewed: 2026-10-20

MyPreflight is a pre-flight briefing and flight-tracking companion for Microsoft Flight Simulator. SimBrief gives the pilot the numbers; MyPreflight adds the real-world context around them and records how the flight went. This repo describes the product as it is now. Git history holds the past.

Reading this by URL? Every path below resolves under `https://raw.githubusercontent.com/mypreflight/product/main/`, e.g. `https://raw.githubusercontent.com/mypreflight/product/main/product/overview.md`. Clone the repo only as a last resort.

## Task routing

| If you're… | Read |
| --- | --- |
| Changing what a feature does | `product/overview.md`, `product/decisions.md`, `features/catalogue.md` |
| Touching the briefing (loadsheet, manifest, cargo, NOTOC, weather) | `product/briefing-content.md`, `product/decisions.md` |
| Touching roles, flight states, delays, Discord or sign-in | `product/platform-rules.md`, `product/accounts-and-privacy.md` |
| Touching points, timing or penalties | `product/scoring.md`, `product/flight-lifecycle.md` |
| Working on the Transponder or tracking | `product/transponder.md`, `product/flight-lifecycle.md` |
| Adding or refreshing data (airports, aircraft, weather, procedures) | `product/data-sources.md`, `product/platform-rules.md` |
| Working on how the product looks | `product/brand/design-rules.md`, `product/brand/identity.md` |
| Touching accounts, privacy, emails or moderation | `product/accounts-and-privacy.md` |
| Working on pricing, competitors or the launch | `product/business.md`, `product/competition.md`, `product/launch.md` |
| Writing a social post, release note or marketing copy | `product/brand/voice.md`, `product/brand/marketing.md`, `product/brand/screenshots.md`, `features/catalogue.md` |
| Proposing something new | `ideas-parking-lot.md` first; it may already be there |

## Rules for every agent

1. **Product and business only.** Never add code, code links, file paths, repo names, routes, API or database details, the tech stack, issue links or infrastructure. Say what the product does and why, in words a pilot could follow. Links between documents in this repo are fine.
2. Decisions here beat assumptions. If the built product contradicts a decision, flag it. Don't silently change either side.
3. Everything in `product/` is decided unless marked **Open:**. Ideas and Claude's suggestions live only in `ideas-parking-lot.md` and are not decided.
4. Never turn an idea or suggestion into a decision. Ask Oskar.
5. This repo is public. No secrets, tokens, internal URLs, IPs, user data or open security weaknesses.
6. When product behaviour changes, update the matching file here in a separate PR as part of the same work.
7. Flight simulation use only. Never write anything suggesting MyPreflight is for real-world training or operations.
8. Determinism first. User-facing aviation data comes from open or properly licensed datasets and deterministic rules. Don't add AI-generated content to it without a decision in `product/decisions.md`.
9. Never judge a pilot on data we can't trust.
10. Plain English, short sentences, aviation terms used correctly.

## Glossary

| Term | Meaning |
| --- | --- |
| OFP | Operational flight plan, imported from SimBrief. The first 2 or 3 pages matter most. |
| Off-block / on-block | Aircraft leaves / arrives at its parking position. Off-block is the only scored time. |
| Transponder | MyPreflight's Windows connector that streams sim data (working name; public name not final) |
| ADS-B over HTTP | Our tracking idea: the Transponder sends position reports over the internet to our own receiver |
| Weight on wheels | Whether the aircraft is on the ground; one of the fields the Transponder sends |
| Gate / stand | Gate is the passenger side, parking stand the aircraft and crew side. The terminal is derived from them. |
| Registration | The real airframe a pilot flies, e.g. SP-LRC (a LOT 787-9) |
| Repositioning | An implied ferry flight when an airframe departs from somewhere other than where it last landed |
| Loadsheet | The document showing how the aircraft is loaded; ours is loosely modelled on Lufthansa's |
| MEL | Minimum equipment list: known defects an aircraft may dispatch with |
| AIP | Aeronautical Information Publication, published by each state; source for curfews and noise rules |
| AIRAC | The 28-day cycle on which aeronautical data is updated |
| CIFP | The FAA's free coded instrument flight procedures (SIDs, STARs, approaches) |

## Who decides

Oskar is the product owner, a licensed pilot and, for now, the only developer. He reviews everything agents produce, including every social post. Questions and proposals: GitHub issues on this repo, or the MyPreflight Discord.
