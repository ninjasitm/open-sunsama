---
applyTo: "**/*"
description: "Development workflows for monorepo."
---

# Workflows

Follow the rules defined in [.claude/rules-snippets/workflows.md](../../.claude/rules-snippets/workflows.md).

Key points:
- **Dev**: `bun run dev:local` (all apps), `--filter=@open-sunsama/api` (specific), `--filter=@open-sunsama/api...` (with dependencies).
- **Build**: `bun run build` (all), `--filter=[origin/main]` (only changed).
- **Adding features**: Determine app/package, check `AGENTS.md`, implement, lint, test, PR with conventional commits.
- **Adding shared code**: Add to `packages/{{PACKAGE_NAME}}/src/`, export from index, add dependency to consuming apps.
