# AGENTS.md - Mobile, Tauri (`apps/mobile`)

Tauri v2 iOS and Android shell around the web app (`apps/web`), with native plugins for haptics, biometrics, deep links, notifications, and store. Parent rules: [../../AGENTS.md](../../AGENTS.md) and [../../CLAUDE.md](../../CLAUDE.md). The Expo app is a separate workspace: see [../expo-mobile/AGENTS.md](../expo-mobile/AGENTS.md).

## Commands

- iOS dev: `cd apps/mobile && bun run dev:ios` (`tauri ios dev`)
- Android dev: `cd apps/mobile && bun run dev:android` (`tauri android dev`)
- Builds: `bun run build:mobile:ios` and `bun run build:mobile:android` from the repository root
- Typecheck: `bunx turbo run typecheck --filter=@open-sunsama/mobile`

## Gotchas

- `src-tauri/tauri.conf.json` sets `devUrl` to a fixed LAN address. Change it to your machine's address before you run on a device.
- Toolchain setup (Xcode, Android SDK, Rust) is in [README.md](README.md).
- Native plugin changes need the same review as desktop capability changes (`src-tauri/capabilities/`).
