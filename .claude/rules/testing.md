---
paths:
  - "**/*.{test,spec}.*"
  - "e2e/**"
  - "mcp/tests/**"
description: "Testing standards, commands, and conventions."
---

# Testing Standards

Follow the rules defined in [.claude/rules-snippets/testing.md](../rules-snippets/testing.md).

Key points:
- Unit tests: `*.test.ts` / `*.test.tsx`; integration: API route tests beside the code; E2E tests: `e2e/*.spec.ts`.
- No coverage threshold is enforced today.
- New features require tests before merging.
