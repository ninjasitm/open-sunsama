---
paths:
  - "apps/**"
  - "packages/**"
  - "mcp/**"
description: "Monorepo logging standards and cross-app tracing."
---

# Logging

Follow the rules defined in [.claude/rules-snippets/logging.md](../rules-snippets/logging.md).

Key points:
- Always prefix logs with `[App:name]` or `[Pkg:name]` for context.
- Use correlation IDs for cross-app request tracing.
- Centralize logging in `packages/utils/src/` once a second consumer needs it (no shared logger exists yet).
- Never log passwords, tokens, or sensitive credentials.
