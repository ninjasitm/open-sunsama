# Coding Standards

## Primary Pattern: Zod-validated Hono routes that delegate to services

```ts
// apps/api/src/routes/*.ts (shape only; check the real route before copying it)
tasksRouter.post("/", auth, requireScopes("tasks:write"), zValidator("json", createTaskSchema), async (c) => {
  const input = c.req.valid("json");
  const task = await createTask(input); // business logic lives in apps/api/src/services/
  return c.json({ success: true, data: task }, 201);
});
```

**Rules:**

- Validate every request body and query with a Zod schema from `apps/api/src/validation/` (`zValidator`).
- Keep route handlers thin; business logic goes in `apps/api/src/services/`.
- Share types and helpers through `@open-sunsama/types` and `@open-sunsama/utils`; do not copy them into apps.

## Data Access Pattern

```ts
// Drizzle through the shared database package (packages/database)
import { getDb, eq, tasks } from "@open-sunsama/database";

const [task] = await getDb().select().from(tasks).where(eq(tasks.id, taskId));
```

## API / Route Pattern

```ts
// Success: { success: true, data, meta? }
// Error:   { success: false, error: { code, message, statusCode } }
import { NotFoundError } from "@open-sunsama/utils";

if (!task) throw new NotFoundError(/* constructor: packages/utils/src/errors.ts */);
```

## Naming Conventions

- Files: kebab-case (`task-series.ts`, `calendar-events.ts`)
- Components: PascalCase names in kebab-case files, grouped in feature folders under `apps/web/src/components/`
- Functions: camelCase; hooks start with `use`
- Variables: camelCase; module constants in UPPER_SNAKE_CASE (`DEFAULT_TIMEZONE`)

## Code Quality

- Follow Hono and React and TypeScript best practices
- Run linting before committing: `bun run lint`
- Format code consistently: `bun run format`

## SRP and reuse-first principle

- **Before implementing new functionality, review existing patterns** in `apps/web/src/hooks/`, `packages/utils/src/`, and `apps/api/src/lib/` for capabilities that already solve the problem.
- **Prefer extending existing utilities** over creating new ones or duplicating logic inline — a new solution is only warranted when the existing one genuinely cannot accommodate the use case.
- **Never duplicate state or behavior** that an existing utility already manages (e.g., loading flags, error state, async wrappers). Inline duplication violates SRP and makes behavior inconsistent across the codebase.
- When in doubt, extend the existing pattern and keep consumers (components, controllers, handlers) thin.
