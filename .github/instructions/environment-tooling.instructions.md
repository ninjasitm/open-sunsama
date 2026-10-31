---
applyTo: "**/*"
description: "Environment and tooling configuration rules for the open-sunsama monorepo."
---

# Environment & Tooling

Follow the rules defined in [.claude/rules-snippets/environment-tooling.md](../../.claude/rules-snippets/environment-tooling.md).

Key points:
- Tailor commands to the user's OS and shell.
- Use `bun` for package management across all apps and packages.
- Start local development with `bun run dev:local`; never start a local API from `apps/api/.env` (it points at production). No Docker for local development or tests.
- Use workspace commands (e.g., `bunx turbo run <task> --filter=@open-sunsama/api`) for app-specific tasks.
