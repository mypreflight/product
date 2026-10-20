# MyPreflight knowledge base

The operating manual for MyPreflight: product decisions, standards, architecture and brand. Start here, then read only the files your task needs.

Last reviewed: 2026-10-20

MyPreflight is a pre-flight briefing and flight-tracking companion for Microsoft Flight Simulator. SimBrief gives the pilot the numbers; MyPreflight adds the real-world context around them and records how the flight went. This repo describes the product as it is now. Git history holds the past.

Reading this by URL? Every path below resolves under `https://raw.githubusercontent.com/mypreflight/product/main/`, e.g. `https://raw.githubusercontent.com/mypreflight/product/main/product/overview.md`. Clone the repo only as a last resort.

## Task routing

| If you're… | Read |
| --- | --- |
| Building or changing a feature | `product/overview.md`, `product/decisions.md`, `features/catalogue.md`, then `frontend/` or `backend/` |
| Touching points, timing or penalties | `product/scoring.md`, `product/flight-lifecycle.md` |
| Working on the Transponder or tracking | `product/transponder.md`, `product/flight-lifecycle.md`, `backend/architecture.md` |
| Adding or refreshing data (airports, aircraft, weather, procedures) | `product/data-sources.md` |
| Doing frontend UI work | `frontend/standards.md`, `frontend/design-rules.md`, `brand/identity.md` |
| Doing backend work in any backend repo | `backend/standards.md`, `backend/architecture.md` |
| Working on infra, deploys or backups | `infrastructure/overview.md` |
| Touching accounts, privacy, emails or moderation | `product/accounts-and-privacy.md` |
| Writing a social post, release note or marketing copy | `brand/voice.md`, `brand/marketing.md`, `brand/screenshots.md`, `features/catalogue.md` |
| Proposing something new | `ideas-parking-lot.md` first; it may already be there |
| Looking for a code repo | `repos.md` |

## Rules for every agent

1. Decisions here beat assumptions. If code contradicts a decision, flag it. Don't silently change either side.
2. Everything in `product/` is decided unless marked **Open:**. Ideas and Claude's suggestions live only in `ideas-parking-lot.md` and are not decided.
3. Never turn an idea or suggestion into a decision. Ask Oskar.
4. This repo is public. No secrets, tokens, internal URLs, IPs, user data or open security weaknesses.
5. When your code change alters product behaviour, update the matching file here in a separate PR as part of the same work.
6. Flight simulation use only. Never write anything suggesting MyPreflight is for real-world training or operations.
7. Determinism first. User-facing aviation data comes from open or properly licensed datasets and deterministic rules. Don't add AI-generated content to it without a decision in `product/decisions.md`.
8. Never judge a pilot on data we can't trust.
9. Plain English, short sentences, aviation terms used correctly.

## Glossary

| Term | Meaning |
| --- | --- |
| OFP | Operational flight plan, imported from SimBrief. The first 2 or 3 pages matter most. |
| Off-block / on-block | Aircraft leaves / arrives at its parking position. Off-block is the only scored time. |
| Transponder | MyPreflight's Windows connector that streams sim data (working name; public name not final) |
| ADS-B over HTTP | Our tracking design: the Transponder sends position reports over HTTP to an in-memory receiver |
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
