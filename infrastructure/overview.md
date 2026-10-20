# Infrastructure

Where MyPreflight runs and how it's kept healthy. No secrets, hostnames or IPs here.

Last reviewed: 2026-10-20

## Where things run

| Component | Runs on | Shipped by |
| --- | --- | --- |
| Main API | DigitalOcean App Platform (one app for the platform) | Docker image pushed to GitHub Container Registry on merge to `main`; the release job pins the app to that version and deploys it |
| ADS-B receiver | DigitalOcean App Platform | Docker image pushed to GitHub Container Registry on merge to `main`. How the new image reaches production is {to confirm}. |
| Provider functions (osm, aerolopa, postcard) | DigitalOcean Functions, as components of the App Platform app (`.do/app.component.yaml` in each repo) | Tagged release on merge to `main` |
| Database | PostgreSQL 16 | Prisma migrations run on deploy |
| Postcard images | DigitalOcean Spaces (object storage) | Written by the postcard generator |
| Web app | GitHub Pages, custom domain set in the repo's Pages settings | Built and deployed by GitHub Actions on merge to `main` |
| Website | GitHub Pages | Built and deployed by GitHub Actions on merge to `main` |
| Transponder | Pilots' PCs; downloads from GitHub Releases | Built with Bun into one Windows executable on a Windows runner. Smoke-tested before release: it starts, draws its dashboard, carries its icon and reaches the notification area. |

Hosting is DigitalOcean only. A move to AWS may happen one day; nothing is planned.

## Environments

- **Production** is the only hosted environment.
- **Local development** runs each backend repo in Docker Compose, with every external provider (SimBrief, weather, ADS-B, Discord, Google, the provider functions) replaced by a mock container loaded from JSON fixtures. Developers never point a local stack at production data.
- Outside production, Discord messages are written to files instead of being sent, and email is observable without real delivery.

## Releases

- Every repo uses semantic versioning. CI refuses a PR whose version is already tagged.
- Merging to `main` tags the version and creates a GitHub release with generated notes.
- Each release produces exactly one deployable artefact: an image, a function bundle, a static site or an executable.

## CI checks

| Repo type | On every pull request |
| --- | --- |
| Backend services | Line endings, file permissions, version; then format, lint, unit and functional tests inside Docker Compose (the receiver also runs `npm audit`) |
| Provider functions | Line endings, file permissions, version; then lint, typecheck, unit and functional tests per function |
| Web app, website | Version check, lint, typecheck (`astro check` on the website), build |
| Transponder | Line endings, file permissions, version; then lint, typecheck, unit tests and an executable build |

## Backups

- Database backups: daily, overnight. Restore has been tested for real.
- Daily backups are enough at the current scale, even at around 50 users; more frequent ones only with more traffic.
- Live positions live only in the receiver's memory. The API copies every active flight's track into the database every 5 minutes, so a receiver restart loses at most about 5 minutes of positions.

## Monitoring

Basic today.

Planned:

- Phone alerts when a service is down.
- A heartbeat check on the 5-minute position backup job.
- External uptime checks.
