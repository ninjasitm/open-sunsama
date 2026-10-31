# @open-sunsama/database

Drizzle ORM schema, client, and migrations for PostgreSQL. Internal to the monorepo.

## Usage

```ts
import { getDb, eq, tasks } from "@open-sunsama/database";

const [task] = await getDb().select().from(tasks).where(eq(tasks.id, taskId));
```

Subpath exports: `@open-sunsama/database/schema`, `/client`, and `/migrate`.

## Development

```bash
# Generate a migration after a schema change, then commit the SQL in drizzle/
bun run db:generate

# Apply migrations (local dev database, via dev:local rules in AGENTS.md)
bun run db:migrate
```

- Migrations: `drizzle/` (starts at `0000_baseline`). Commit the generated SQL with the schema change.
- `db:push` is for scratch databases only. It skips the migration history.
- Never change migrations that have already shipped; add a new one.
- Rules: [../../AGENTS.md](../../AGENTS.md), Database section.
