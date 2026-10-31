# @open-sunsama/api-client

Typed HTTP client and React Query hooks for the Open Sunsama API. Internal to the monorepo.

## Usage

```ts
import {
  createOpenSunsamaClient,
  createTasksApi,
} from "@open-sunsama/api-client";

const client = createOpenSunsamaClient({
  baseUrl: "http://localhost:3001",
  token,
});
const tasks = await createTasksApi(client).list({ completed: false });
```

React hooks live under the `@open-sunsama/api-client/react` subpath.

## Development

```bash
bunx turbo run test --filter=@open-sunsama/api-client
bunx turbo run typecheck --filter=@open-sunsama/api-client
```

- One module per API area (`tasks.ts`, `time-blocks.ts`, `subtasks.ts`, and so on). Keep them in step with the routes in `apps/api/src/routes/`.
- Source: `src/`.
