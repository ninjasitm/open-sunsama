---
applyTo: "**/*"
description: "API route patterns, input validation, and response formats for the open-sunsama monorepo."
---

# API Server

Follow the rules defined in [.claude/rules-snippets/api-server.md](../../.claude/rules-snippets/api-server.md).

Key points:
- Hono route modules live in `apps/api/src/routes/` (one file per resource); validate all input data before processing.
- Return consistent response formats with proper HTTP status codes.
- Document API endpoints in `docs/api/` and the Routes table in AGENTS.md.
- Use shared API types from `packages/` for cross-app consistency.
