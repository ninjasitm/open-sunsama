# @open-sunsama/utils

Shared utilities: date helpers, validation, error classes, API-key helpers, and constants. Internal to the monorepo.

## Usage

```ts
import { NotFoundError, uuidSchema } from "@open-sunsama/utils";
```

Subpath exports: `@open-sunsama/utils/date`, `/validation`, `/errors`, `/api-key`, and `/constants`.

## Development

```bash
bunx turbo run test --filter=@open-sunsama/utils
bunx turbo run typecheck --filter=@open-sunsama/utils
```

- Coverage is configured in `vitest.config.ts`; no threshold is enforced.
- Source: `src/`, with a `*.test.ts` file next to each module.
- Changes here reach the API and the web app. Run their tests too.
