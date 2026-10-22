# Repositories

Every MyPreflight code repo and what it does.

Last reviewed: 2026-10-20

All code repos are public on GitHub. Short names are used across this knowledge base. Each repo also has its own `README.md`; most have an `AGENTS.md`.

| Short name | Repo | Purpose | Stack |
| --- | --- | --- | --- |
| `app` | [oskarbarcz/flight-tracker-app](https://github.com/oskarbarcz/flight-tracker-app) | The web app at app.mypreflight.io: dispatch, briefing, tracking, history, airport library | React 19, React Router (SPA), Vite, Flowbite React, Tailwind v4, Leaflet, PWA |
| `api` | [oskarbarcz/flight-tracker-api](https://github.com/oskarbarcz/flight-tracker-api) | Main API, the Discord bot and scheduled jobs | NestJS 11, CQRS, Prisma 7, PostgreSQL 16, Socket.IO |
| `adsb` | [oskarbarcz/adsb-receiver-api](https://github.com/oskarbarcz/adsb-receiver-api) | ADS-B over HTTP receiver: positions in memory, per callsign | NestJS, cache-manager store, no database |
| `transponder` | [mypreflight/transponder-app](https://github.com/mypreflight/transponder-app) | The Windows connector: position feed from MSFS 2024, Discord rich presence, updates | TypeScript on Node, compiled to one .exe with Bun; `node-simconnect` |
| `osm` | [mypreflight/osm-provider](https://github.com/mypreflight/osm-provider) | Reads one airport's infrastructure from OpenStreetMap for the API's upgrade review | Dependency-free TypeScript, DigitalOcean Functions |
| `aerolopa` | [mypreflight/aerolopa-provider](https://github.com/mypreflight/aerolopa-provider) | Seat maps, cabin configurations and the layout index from AeroLOPA | Dependency-free TypeScript, DigitalOcean Functions |
| `postcard` | [mypreflight/postcard-generator](https://github.com/mypreflight/postcard-generator) | One travel-poster postcard per city | Dependency-free TypeScript, DigitalOcean Functions, OpenAI image API, DigitalOcean Spaces |
| `landing` | [mypreflight/landing](https://github.com/mypreflight/landing) | The mypreflight.io website | Astro (static, no UI framework), Tailwind v4 |
| `etl-tools` | [oskarbarcz/flight-tracker-etl-tools](https://github.com/oskarbarcz/flight-tracker-etl-tools) | Scripts that sync reference data (operators so far) into the API, plus airline fin and aircraft illustration generators | TypeScript scripts on Node, no build step |
| `.github` | [mypreflight/.github](https://github.com/mypreflight/.github) | The organisation's GitHub profile | — |

## Notes

- **Discord bot:** no separate repo. The bot lives in `api`; rich presence is published by `transponder`.
- **Integrations and webhooks:** AeroLOPA, OpenStreetMap and postcards each have a function repo above. SimBrief, weather (aviationweather.gov, SayIntentions), SkyLink, Google, Discord and email are clients inside `api`.
- **Owners:** newer repos live in the `mypreflight` organisation; `app`, `api`, `adsb` and `etl-tools` are still under `oskarbarcz`.
- **Licence:** the code repos are under The Unlicense, except `landing` and `etl-tools`, which carry no licence file. See `product/decisions.md` → Code licence.
- **Project board:** [MyPreflight Development board](https://github.com/users/oskarbarcz/projects/12) tracks issues across repos.
