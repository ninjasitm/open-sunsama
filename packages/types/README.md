# @open-sunsama/types

Shared TypeScript types for the API, the web app, and the Expo app. Internal to the monorepo.

## Usage

```ts
import type { Task } from "@open-sunsama/types";
```

Subpath exports: `@open-sunsama/types/user`, `/task`, `/time-block`, `/api-key`, `/api`, and `/common`.

## Development

```bash
bunx turbo run typecheck --filter=@open-sunsama/types
```

- A change here changes the contract for every consumer. Check the API, `@open-sunsama/api-client`, and the apps that import the type.
- Source: `src/` (one file per domain).
