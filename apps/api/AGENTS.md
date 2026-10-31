# AGENTS.md - API (`apps/api`)

Hono v4 REST API and remote MCP endpoint (`/mcp`), port 3001. Parent rules: [../../AGENTS.md](../../AGENTS.md) (testing, shipping, Railway) and [../../CLAUDE.md](../../CLAUDE.md).

## Commands

Run from the repository root unless noted.

- Local dev (safe): `bun run dev:local`. It sets every variable explicitly and refuses the production database.
- Test: `bunx turbo run test --filter=@open-sunsama/api` (Vitest)
- Typecheck: `bunx turbo run typecheck --filter=@open-sunsama/api`
- Build: `bunx turbo run build --filter=@open-sunsama/api` (tsup)
- Migrations: `bun run db:generate` and `bun run db:migrate`. The API applies migrations on start only when `MIGRATE_ON_START=true`.

Never start a local API from `apps/api/.env`. That file points at production, and `src/index.ts` loads it with `dotenv/config`.

## Structure

- `src/index.ts`: app setup, CORS (`CORS_ORIGIN`), route registration, `/health`
- `src/routes/`: one file per resource (`tasks`, `time-blocks`, `calendar-events`, `mcp`, `oauth`, and so on)
- `src/validation/`: Zod schemas per resource
- `src/middleware/`: `auth` (JWT, API key, MCP OAuth token) and `requireScopes`
- `src/services/`: business logic that routes call
- `src/lib/`: helpers (WebSocket, PG Boss, timers, web push, OAuth)
- `src/workers/`: background jobs
- `src/migrate.ts`: migration runner (`node dist/migrate.js`)

## Patterns

- Routes: a Hono router per resource, `zValidator` with schemas from `src/validation/`, `auth` plus `requireScopes(...)`, and JSON responses in the envelope from AGENTS.md (API Response Format).
- Errors: throw the error classes from `@open-sunsama/utils`.
- Data: Drizzle through `getDb()` from `@open-sunsama/database`. Schema and migrations live in `packages/database`.
- Scopes: `tasks:read|write`, `time-blocks:read|write`, `calendar:read`, `user:read|write`.
- Background jobs: PG Boss (`src/lib/pgboss.ts`). `/health` reports `pgBossRunning`.

## Gotchas

- Changing a route path, a response shape, or a scope is an API contract change. Ask first, then update `docs/api/`, `packages/api-client`, and the web app that calls it.
- Shared types come from `@open-sunsama/types`. Change them there, then check the API, `api-client`, and the web app.
- Auth flows (`/auth`, `/oauth`) are not changed without confirmation.

## Related

- [README.md](README.md)
- [../../packages/database/](../../packages/database/) (schema and migrations)
- [../../docs/api/](../../docs/api/) (endpoint docs)
