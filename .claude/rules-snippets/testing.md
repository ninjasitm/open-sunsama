## Commands

```bash
# All tests
bun run test

# Specific app/package tests
bunx turbo run test --filter=@open-sunsama/api

# Watch mode
bunx turbo run test --filter=@open-sunsama/api -- --watch
```

## Test Locations

### Unit Tests

- Located in each package: `packages/**/*.test.ts`
- Located in each app: `apps/**/*.test.ts` and `apps/**/*.test.tsx`
- Run with: `bun run test`

### Integration Tests

- No separate integration suite yet. API route tests sit beside the code (`apps/api/src/**/*.test.ts`) and run with `bun run test`.

### E2E Tests

- Located in: `e2e/` at the repo root (Playwright; runs against the stack from `e2e/start-stack.sh`)
- Run with: `bun run e2e`

## Coverage Requirements

- No coverage threshold is enforced today
- Critical paths must have integration or E2E coverage
- New features require tests before merging

## Cross-Package Testing

- Shared packages should have standalone unit tests
- Apps should test integration with shared packages
- Changes to shared packages must pass all downstream app tests
