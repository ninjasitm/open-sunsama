---
applyTo: "**/*"
description: "Testing and quality assurance standards for the open-sunsama monorepo."
---

# Testing & Quality

Follow the rules defined in [.claude/rules-snippets/testing-quality.md](../../.claude/rules-snippets/testing-quality.md).

Key points:
- Ensure lint, test, and build pass before committing.
- Use Vitest for unit/component tests and Playwright for end-to-end tests.
- Place tests alongside implementation files or in `tests/` directory.
- Aim for meaningful test coverage of business logic; shared packages need standalone unit tests.
- Run `bun run test` for all apps or `--filter=@open-sunsama/api` for specific apps.
