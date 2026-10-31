---
applyTo: "**/*"
description: "Monorepo patterns and conventions."
---

# Monorepo Patterns

Follow the rules defined in [.claude/rules-snippets/patterns.md](../../.claude/rules-snippets/patterns.md).

Key points:
- **Package naming**: Apps use direct names, packages use `@open-sunsama/` scope. Use `@/` for internal app imports.
- **SRP and reuse-first principle** — Review existing shared packages before implementing new functionality; never duplicate state or behavior that a utility already manages.
- **Turbo tasks**: Use `bunx turbo run build --filter=` for specific workspaces; `^build` for dependency ordering.
- Shared config lives at the repo root (`eslint.config.mjs`, `tsconfig.base.json`); each app extends it.
