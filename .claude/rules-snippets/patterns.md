## Workspace Conventions

### Package Naming

- Apps: Direct name (e.g., `web`, `api`, `docs`)
- Packages: Scoped (e.g., `@open-sunsama/ui`, `@open-sunsama/utils`)

### Import Paths

```typescript
// From shared packages
import { Button } from "@open-sunsama/ui";

// Internal app imports
import { Component } from "@/components";
import { utils } from "~/utils";
```

## Turbo Tasks

### Task Dependencies

```json
{
	"tasks": {
		"build": {
			"dependsOn": ["^build"],
			"outputs": ["dist/**"]
		},
		"dev": {
			"cache": false,
			"persistent": true
		}
	}
}
```

### Running Tasks

```bash
# All workspaces
bun run build

# Specific workspace
bunx turbo run build --filter=@open-sunsama/api

# Workspace and dependencies
bunx turbo run build --filter=@open-sunsama/api...
```

## SRP and reuse-first principle

- **Before implementing new functionality, review existing patterns** in shared packages (`packages/utils/src/`, `apps/api/src/lib/`, `apps/web/src/hooks/`) for capabilities that already solve the problem.
- **Prefer extending existing utilities** over creating new ones or duplicating logic inline — a new solution is only warranted when the existing one genuinely cannot accommodate the use case.
- **Never duplicate state or behavior** that an existing utility already manages (e.g., loading flags, error state, async wrappers). Inline duplication violates SRP and makes behavior inconsistent across the monorepo.
- When in doubt, extend the existing pattern and keep consumers (components, controllers, handlers) thin.
- Reusable logic belongs in a shared `packages/` module, not copied across apps.

## Shared Configuration

### ESLint

```javascript
// eslint.config.mjs (repo root) is the shared base; each workspace inherits it.
```

### TypeScript

```json
// apps/api/tsconfig.json
{
  "extends": "../../tsconfig.base.json"
}
```

## Adding New Packages

1. Create directory in `packages/` or `apps/`
2. Add `package.json` with proper scope
3. Update monorepo config if needed
4. Add to consuming packages

## Adding Dependencies

```bash
# To specific app/package
bun add <package> --filter=@open-sunsama/api

# To root workspace
bun add -D <package> -w

# Internal package dependency
bun add @open-sunsama/utils --filter=@open-sunsama/api
```

## Code Sharing Patterns

### Shared Package Usage

```typescript
// packages/utils/src/index.ts
export { Component } from "./Component";
export type { ComponentProps } from "./types";

// apps/api/src/pages/index.tsx
import { Component } from "@open-sunsama/utils";
```

### Configuration Sharing

```typescript
// eslint.config.mjs (repo root) is the shared base; each workspace inherits it.
```
