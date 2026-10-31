---
paths:
  - "**/*.{test,spec}.*"
  - "e2e/**"
  - "mcp/tests/**"
description: "Testing standards and quality assurance."
---

# Testing Quality

Follow the rules defined in [.claude/rules-snippets/testing-quality.md](../rules-snippets/testing-quality.md).

Key points:
- Ensure lint, test, and build pass before committing.
- Use Vitest for unit/component tests and Playwright for end-to-end tests.
- Place tests alongside implementation files or in `tests/` directory.
- Aim for meaningful test coverage of business logic.
