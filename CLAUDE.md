# open-sunsama

@AGENTS.md

AGENTS.md is the source of truth for project rules (testing, shipping, releases, Railway). This file only adds the commands and layout you need at a glance.

## Commands

- Install: `bun install`
- Dev (local, no Docker): `bun run dev:local` (API on :3001, web on :3000; set `API_PORT` / `WEB_PORT` to run several checkouts side by side)
- Build: `bun run build`
- Test: `bun run test`
- Lint: `bun run lint`
- Typecheck: `bun run typecheck`
- E2E: `bun run e2e` (needs the stack from `e2e/start-stack.sh`)

Never run `bun dev` for local work: it starts the API from `apps/api/.env`, which points at production.

## Stack

- TypeScript: Hono v4 (API), React 19 + Vite (web), Tauri v2 (desktop and mobile shells), Expo (mobile)
- Database: PostgreSQL with Drizzle ORM
- Testing: Vitest (unit), Playwright (E2E in `e2e/`)
- Build system: Turborepo
- Package manager: Bun 1.4.2

## Apps

- `apps/api` — Hono REST API and remote MCP endpoint (port 3001)
- `apps/web` — React 19 + Vite SPA (port 3000)
- `apps/desktop` — Tauri v2 desktop wrapper of the web app
- `apps/mobile` — Tauri v2 iOS and Android wrapper of the web app
- `apps/expo-mobile` — Expo (React Native) mobile app

## Packages

- `packages/database` — Drizzle schema, migrations, and `getDb`
- `packages/types` — shared TypeScript types
- `packages/api-client` — typed HTTP client and React Query hooks
- `packages/utils` — date, validation, and error helpers
- `mcp/` — `@open-sunsama/mcp`, the MCP server for AI agents (workspace package)

## Claude Code

- Rules are in `.claude/rules/` (thin wrappers) and their source is in `.claude/rules-snippets/`. Rules load automatically each session.
- Agent definitions are in `.claude/agents/` (source: `.claude/agents-snippets/`).
- Custom commands are in `.claude/commands/`.
- Skills are in `.agents/skills/` and `.claude/skills/`.
- See `AGENTS.md` for project context, the orchestration checkpoint, and the agent hierarchy.
- For cross-app changes, use the orchestrator-first flow with the Feature Builder coordinator.

## GitHub Copilot

- Instructions are in `.github/instructions/`, scoped by `applyTo`; context is in `.github/copilot-instructions.md`.
- Agents are in `.github/agents/`. Prompts are in `.github/prompts/`.
- No development MCP server is configured (`.vscode/mcp.json` is absent).

## Cursor IDE

- Rules are in `.cursor/rules/` (.mdc). Agents are in `.cursor/agents/`. Commands are in `.cursor/commands/`.
- No development MCP server is configured (`.cursor/mcp.json` is absent).

## OpenCode

- Config is in `.opencode/opencode.jsonc`. Commands are in `.opencode/commands/`. Rules are in `.opencode/rules/`. Agents are in `.opencode/agents/`.
- Native V2 loads the ambient `AGENTS.md`. Explicitly read the snippets a rule references, because `@refs` are plain text.
- Skills load from `.agents/skills/` and `.claude/skills/`.

## Codex

- Reads `AGENTS.md` and `.agents/skills/` natively. `.codex/config.toml` is comment-only, so your Codex defaults apply; project config loads only after you trust the project.
