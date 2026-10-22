# Ideas parking lot

Undecided ideas and Claude's unconfirmed suggestions. **Nothing in this file is decided.**

Last reviewed: 2026-10-20

Add ideas from chats, GitHub issues and notes here. Move an item into `product/` only when Oskar decides it.

| Label | Meaning |
| --- | --- |
| Idea | Oskar's idea, not decided |
| Claude suggestion | Proposed by Claude, **not confirmed by Oskar** |

## Oskar's ideas

Every item in this section is labelled **Idea**.

### Briefing and data

- OFP reading help: hover tooltips for OFP abbreviations, plus docs on reading a SimBrief OFP (the first 2 or 3 pages matter most). Doubles as marketing.
- "Don't know SimBrief?" pop-up pointing to tutorials.
- Plain-language NOTAM translation as a paid add-on.
- Collect fuel-burn data now to detect per-aircraft bias later (e.g. the iniBuilds A350 burning about 1% more than its SimBrief profile) and suggest extra fuel next time. Share aggregated insights with SimBrief or add-on developers such as iniBuilds, to build relationships.
- Commercial aircraft-registration APIs (e.g. JetPhotos). Heavy work and agreements; not soon.
- Official AIP charts through a source like ChartFox or MSFS 2024's Lido charts. Terms unknown.
- Keep historic airlines such as Alitalia; some simmers want to fly them.
- Airline-specific loadsheet and flight-plan layouts, given format input.

### Tracking and events

- "Track any flight" mode: tracking only, no briefing, for aircraft outside the scope. Low priority.
- Automatic matching of flights to VATSIM events, once there are enough users.
- After a diversion, prompt the pilot to continue onward or ferry back.
- Bonus for landing at the declared diversion airport or making a good emergency landing.
- Automated community stats (busiest airport, busiest route).

### Collectibles and profile

- Virtual type ratings: a short course per aircraft type (e.g. 10 sentences on operating the 777, then about 5 questions; a few hundred questions in total), shown as a certificate or badge on the profile. Content from AI, checked by airline experts or the community. Alternatively earned by experience, e.g. 25 flights on type.
- Yearly postcard editions, so pilots revisit cities to collect the new one.
- Physical postcards mailed to subscribers.
- Theme that follows daylight at the aircraft's position (bright while flying over LA, even when it's night at home). Build it, ship it, ask users.

### Platform

- Virtual-airline integration (phpVMS, vAMSYS), both ways, e.g. combining VA PIREPs with MyPreflight.
- Facebook login.
- Marketing emails, e.g. win-back for lapsed subscribers with a discount; a newsletter.
- An installer for the Transponder, with extra features.
- In-app feedback system.
- Funnel and Hotjar-style UX analytics.
- Native mobile apps. Far future at most.
- X-Plane, German and Polish: only on demand.

### From GitHub issues

Open issues that aren't in progress on the [development board](https://github.com/users/oskarbarcz/projects/12). Work already in progress is tracked in `features/catalogue.md`.

- **Aircraft technical logbook** (OpenSpec change `add-aircraft-technical-logbook` in the backend). Informational only; nothing blocks dispatch.
  - [api#151](https://github.com/oskarbarcz/flight-tracker-api/issues/151) ATA chapter reference data: a curated ATA-chapter taxonomy used when a defect is filed.
  - [api#153](https://github.com/oskarbarcz/flight-tracker-api/issues/153) Aircraft utilization ledger: total time since new, block time and cycles per airframe, accrued as flights complete.
  - [api#155](https://github.com/oskarbarcz/flight-tracker-api/issues/155) Defect reporting: pilots who have flown an aircraft report defects against the ATA taxonomy.
  - [api#156](https://github.com/oskarbarcz/flight-tracker-api/issues/156) Defect triage: operations confirm, reject or resolve reported defects, with a classification tied to the aircraft's ETOPS threshold.
  - [api#157](https://github.com/oskarbarcz/flight-tracker-api/issues/157) Technical status and logbook feed: whether the aircraft is serviceable and ETOPS-ready, plus a chronological logbook.
- [app#189](https://github.com/oskarbarcz/flight-tracker-app/issues/189) Move airport management to the admin user.

## Claude's suggestions (not confirmed by Oskar)

Every item in this section is labelled **Claude suggestion** and is not decided.

### Transponder and download page

- Code-sign the .exe, publish checksums, and put "open source, read the code" next to the download button.
- The updater verifies the new file's signature or checksum before telling the user to run it.
- The Transponder sends its version with every request, so the server can refuse very old versions once a breaking change comes.
- Tell users that old versions in Downloads can be deleted.
- Keep "Transponder" as the public name and use it everywhere (download page, app, support, file name), with a one-line explanation for non-geeks. Download-page line: "Your own transponder, running on your PC."
- Three distinct connection states:
  - "your browser paused updates" (reconnect quietly when the tab is focused again);
  - "Transponder silent, waiting" (grace period with a visible countdown);
  - "sim stopped responding" (urgent; the Transponder is still online but the sim is gone, which is how it can tell a crash from an internet drop).
- Shorter grace period on the ground and in taxi, longer in cruise, where a few minutes of gap matter least.
- Show manual off-blocks visibly on the flight page.
- Note: many European airports (e.g. Frankfurt, Heathrow) want Mode S on at the gate before pushback is requested. The "after doors closed" rule fits that.
- Download page plan:
  - a hero line such as "Track your flights. Zero impact on your sim.";
  - one download button showing version, size and Windows 10/11, free, no installer;
  - trust badges: about 20 MB, not Electron, open source, dependencies kept updated;
  - **the most important part:** a plain "what it reads / what it doesn't read" block;
  - three setup steps with a real console screenshot;
  - checksum and signing info;
  - FAQ: Windows SmartScreen warning, VATSIM, MSFS 2020 vs 2024, uninstalling = deleting the file;
  - footer with release notes, Discord support and the sim-only disclaimer.

### Accounts, privacy and moderation

- Let pilots choose flight visibility during onboarding; never flip existing flights silently when the default changes to public.
- Warn on the delete screen that the email address can't be reused.
- Use a keyed hash (HMAC) for deleted emails, not a plain hash.
- Generated avatars instead of photo uploads.
- A display name or callsign option, with the real name optional.
- Nickname guards: reject URLs, multilingual blocklists (Polish included), block registration-shaped names, a report button.
- An unticked marketing opt-in at sign-up.
- Automated CSAM scanning for any uploads (e.g. Cloudflare's tool).

### Product and data

- Cargo realism mix: iconic loads (iPhones out of Zhengzhou) plus everyday ones, so it doesn't feel like a theme park.
- Keep sponsored points off the main leaderboard.
- "For flight simulation use only" in the footer, on briefings and in the terms.
- NOTAM translation as the first post-launch feature, cached per NOTAM ID, with rules for the common patterns before any AI call.
- SIDs: on a runway change show a dashed direct line; after takeoff the real track replaces the planned SID; build a library of runway and SID pairs from imports, expiring per AIRAC.
- Ingest FAA CIFP (a zip per AIRAC cycle in ARINC 424 format) the same way the OSM updater works.
- The SimBrief navlog may include SID and STAR fixes with coordinates; check before building a procedure database.
- Seed registrations from the OpenSky Network database; take type and operator from open data, not SimBrief; reject mismatches.
- For curfews and noise abatement, start from AIP sections AD 2.20 and AD 2.21; do the top 50 airports semi-manually.
- Ask ChartFox about charts; never rehost AIP charts.
- Confirm AeroLOPA's licence before launch.

### Business

- Licence: AGPL for the code plus a registered trademark on the name.
- Check Stripe Tax or a merchant of record for EU VAT; offer Przelewy24 and BLIK for Polish users.
- A founding-member badge for early subscribers.

### Beta and marketing

- Hide tiny live-pilot counts during the beta.
- Beta metric: pilots flying a second flight within a week.
- Plausibility checks against cheating: no teleports, no impossible speeds, fuel must go down.
- OFP explainer posts as a marketing series.
- One release note per release, from which agents generate the in-app note, the changelog, the Discord post and social posts. Keep replies to people personal.
- If the Discord bot ever needs interaction, use buttons and slash commands, no AI (e.g. "I'm back, all good" on a monitoring alert).
- Use the SunCalc library for the daylight theme.

### Operations

- Free uptime checks on health endpoints (DigitalOcean, UptimeRobot or Better Stack).
- A Healthchecks.io heartbeat on the 5-minute position backup job.
- Watch disk space, SSL expiry and database connections.
- Check whether the managed database offers point-in-time recovery.
- A weekly off-site database dump (e.g. Backblaze).
- Infrastructure as code. Today the App Platform app spec lives only in DigitalOcean, each provider function's spec fragment is merged in by hand, and the database and Spaces bucket aren't in code. Two options:
  - commit the full app spec to one repo and let the deploy action apply it on release (the smallest step; the database and Spaces stay manual);
  - describe the app, database, Spaces and domains in Terraform or OpenTofu with DigitalOcean's official provider, importing what already exists.

  Either way, secrets come from GitHub secrets at deploy time, never from the files, because the repos are public.
