# AGENTS.md - Desktop (`apps/desktop`)

Tauri v2 desktop app. It wraps the web app (`apps/web`), so most UI changes happen there. Parent rules: [../../AGENTS.md](../../AGENTS.md) and [../../CLAUDE.md](../../CLAUDE.md).

## Commands

- Shell dev: `cd apps/desktop && bun run dev` (`tauri dev`). It starts the web dev server on port 3000 itself, so do not run `dev:local` at the same time.
- API work: use `bun run dev:local` from the root. Do not use `bun run dev:desktop`: it starts the API from `apps/api/.env`, which points at production.
- Build: `bunx turbo run build --filter=@open-sunsama/desktop` (type check, then `tauri build`, after the web build).
- Release: `bun run release` from the root. It bumps and syncs versions, merges through a PR, and tags. Never tag by hand.

## Structure

- `src/`: desktop-only TypeScript (linted with `eslint src/`)
- `src-tauri/`: Rust shell. `tauri.conf.json`, `Cargo.toml`, `capabilities/`, and `Entitlements.plist` (macOS network entitlement)
- `scripts/prepare-frontend.mjs`: copies the web build into the bundle and fails above 60 MB

## Gotchas

- Hardened-runtime macOS builds need `Entitlements.plist` embedded during signing, or network calls fail (AGENTS.md, Tauri v2 Entitlements).
- Cross-origin API calls go through `@tauri-apps/plugin-http` (`desktopFetch` in `apps/web/src/lib/api.ts`).
- The updater URL `/releases/update/:target/:current_version` must keep its shape. Installed apps depend on it (AGENTS.md, Desktop Auto-Update).
- `VITE_API_URL` and `VITE_WS_URL` must be passed on the Tauri build step itself.
- The signing key password cannot be empty (GitHub Actions).

## Related

- [README.md](README.md)
- [../web/AGENTS.md](../web/AGENTS.md)
