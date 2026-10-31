---
applyTo: "**/*"
description: "Testing strategy and conventions for the open-sunsama monorepo."
---

# Testing Standards

Follow the rules defined in [.claude/rules-snippets/testing.md](../../.claude/rules-snippets/testing.md).

Key points:
- **Commands**: `bun run test` (all), `--filter=@open-sunsama/api` (specific app), `-- --watch` (watch mode).
- **Test locations**: Unit tests in `packages/**/` and `apps/**/`; integration: API route tests beside the code; E2E in `e2e/`.
- **Coverage**: no threshold enforced today; critical paths need integration or E2E coverage; new features require tests before merging.
- **Cross-package testing**: Shared packages need standalone unit tests; apps must test integration with shared packages.
