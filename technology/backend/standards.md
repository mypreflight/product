# Backend standards

Conventions shared by every MyPreflight backend repo.

Last reviewed: 2026-10-20

Applies to the API (`api`), the ADS-B receiver (`adsb`) and the provider functions (`osm`, `aerolopa`, `postcard`); see `../../repos.md`. Repo-specific detail stays in each repo's `CLAUDE.md`.

## Principles

- **Don't break the field.** Design APIs so they don't need breaking changes: the Transponder on pilots' PCs depends on them, and old versions keep running.
- **Cheap by design.** Prefer designs that fit a self-funded project. The ADS-B receiver keeps positions in memory, not in a database.
- **Isolate the brittle and the expensive.** Anything that scrapes, calls a slow or paid service, or depends on how well third-party data is mapped runs as its own function. A failure there degrades one feature, never the whole API.
- **Read-only providers.** Provider functions hold no API credentials and write nothing; the API decides what to store.

## All backend repos

- **TypeScript on Node.** NestJS for long-running services; dependency-free TypeScript for functions.
- **Docker for development.** Never run `npm` or `npx` on the host. Everything runs inside the compose service, which installs, generates, migrates and seeds on start. External providers are replaced by mock containers loaded from JSON fixtures.
- **Semantic versioning.** The version in `package.json` must be bumped in every PR; CI fails when the version is already tagged. Merging to `main` tags the version and creates a GitHub release.
- **CI on every PR** checks line endings (no CRLF), file permission changes and the version, then runs format, lint, and unit and functional tests inside the compose stack. The receiver also runs `npm audit`.
- **Tests:** Jest for unit tests beside the code (`*.spec.ts`); cucumber-js for functional tests in `features/`, written in Gherkin by domain.
- **Specs:** OpenSpec, with current capabilities in `openspec/specs/` and proposals in `openspec/changes/`. The product rules distilled from them are in `../../product/briefing-content.md` and `../../product/platform-rules.md`.
- **No comments that explain a decision, a workaround or a policy.** Put the reasoning in the PR. Keep comments a tool requires.
- **No secrets in the repo.** Configuration comes from the environment; `.env.dist` holds development placeholders only.

## NestJS services (API, ADS-B receiver)

### Structure

Each feature module under `src/modules/<name>/` follows the same split:

```text
application/
  command/   CQRS command handlers (imperative; return ids or nothing)
  query/     CQRS query handlers (read models)
  assert/    "assert X exists" handlers
  event/     @OnEvent listeners (internal/ and external/)
infra/
  database/  repositories, the only place the database client is injected
  http/action/  one controller class per endpoint, plus request DTOs
  service/   glue to external providers and scheduled (@Cron) work
model/       domain types, API-shaped classes, error/*.error.ts
<name>.module.ts
```

- **One controller per endpoint** ("actions"). An action translates HTTP into a `CommandBus` or `QueryBus` call and nothing more. Write-then-read is normal: create, then query by id.
- Every handler and action must be registered in its module; nothing is auto-discovered.
- The application layer never touches the database client directly. Cross-module reads go through the bus to the owning module.
- Repositories fetch one entity with `findById(id)` or `findOneBy(criteria)`.
- Cross-cutting building blocks live in `src/core/` (auth guards and decorators, validation, response DTOs, provider clients, errors, cache, utils). Reach for them before writing a bespoke helper.

### HTTP

- REST under `/api/v1`, documented with Swagger/OpenAPI generated from the code. A field drawn from a closed set is published as that set; a nullable field is published with its type.
- Every endpoint requires a JWT by default; opt out with `@SkipAuth()`. Gate by role with `@Role(...)`, using the domain enum, never the database client's enum.
- Request DTOs are validated with `class-validator` through one global pipe; unknown properties are rejected.
- Read configuration through `ConfigService` (`getOrThrow` for required keys). Only bootstrap code reads `process.env`.

### Errors

- Throw typed domain errors that extend one category: `BadRequestError` (400), `UnauthorizedError` (401), `ForbiddenError` (403), `NotFoundError` (404), `ConflictError` (409), `UnprocessableError` (422), `BadGatewayError` (502).
- Define concrete errors per module in `model/error/*.error.ts`, e.g. `FlightNotFoundError extends NotFoundError`.
- One global filter maps them to one canonical body: `{ statusCode, error, message, violations? }`. The 422 reason phrase is `Unprocessable Content`.
- Never throw framework HTTP exceptions from domain, application or infra code. Only the auth guards and the validation pipe produce framework errors.

### Data

- PostgreSQL through Prisma. The generated client goes to `prisma/client/` and is imported from `prisma/client/client`.
- Migrations with `prisma migrate`. Seed data lives in `prisma/seed/`; resource fixtures are reused by the functional test suite.
- Airport, terminal and gate geometry in seeds comes from OpenStreetMap; never invent coordinates.

### Async work

- Domain events through the event emitter; scheduled jobs with `@Cron`.
- Email delivery never blocks or fails the request that triggered it, and message content is never logged in full.

### Provider clients

- Every external call goes through one fetch helper with a timeout and retries.
- Contracts with our own functions are vendored as OpenAPI files, with hand-written types that mirror them. When a contract changes, update both together; there is no codegen.
- Upstream failures map to typed errors. Anything unexpected collapses into one "unavailable" error per provider, so callers branch on a small set of outcomes.

### Security-sensitive rules

- Third-party sign-in resolves users by the provider's ID only, never by email.
- Authentication identifiers (Google, Discord) are never accepted in request DTOs; only a completed OAuth exchange writes them.
- An account may have no password, so every credential path must handle that before hashing.

## Provider functions (`osm`, `aerolopa`, `postcard`)

- Serverless functions on DigitalOcean Functions, deployed as part of the platform's App Platform app.
- No runtime dependencies: standard library plus compiled TypeScript, for a small artefact and an instant cold start.
- Each request carries a shared secret header; without it the function answers `401`.
- Each repo publishes an `openapi.json` contract, which the API vendors.
- Dockerised development with a mock of the upstream (e.g. an Overpass mock for `osm`).
- Cache aggressively where the upstream changes rarely (seat maps change a few times a year).

## Other TypeScript repos

- **Transponder** (desktop): layered as domain, application (with ports), infrastructure and presentation. Imports run downward only, and an architecture test fails the suite on violations. Tests sit beside the code. See `architecture.md`.
- **ETL tools**: plain scripts run with Node's native type stripping, no build step. Syncs are reconciliations: dry-run by default, matched by a natural key (e.g. ICAO code), minimal non-destructive updates.
