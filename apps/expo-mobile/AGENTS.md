# AGENTS.md - Expo mobile (`apps/expo-mobile`)

Expo SDK app with Expo Router. It uses `@open-sunsama/api-client` and `@open-sunsama/types`. Parent rules: [../../AGENTS.md](../../AGENTS.md) and [../../CLAUDE.md](../../CLAUDE.md).

## Commands

- Dev server: `cd apps/expo-mobile && bun run dev` (`expo start`)
- Native run: `bun run ios` or `bun run android` (`expo run:ios` and `expo run:android`)
- Builds and submit: `bun run build:ios` / `bun run build:android` and `bun run submit:ios` / `bun run submit:android` (EAS)
- Typecheck: `bunx turbo run typecheck --filter=@open-sunsama/expo-mobile`
- Lint: `bunx turbo run lint --filter=@open-sunsama/expo-mobile`

## Structure

- `app/`: Expo Router screens. `(auth)` holds login and register. `(app)` holds the signed-in tabs (tasks, calendar, settings).
- `src/components`, `src/hooks`, `src/lib`: shared mobile code
- `app.json` and `eas.json`: Expo and EAS config. Bundle ID `app.opensunsama.mobile`.

## Gotchas

- Store tokens with `expo-secure-store`. Never log tokens or other credentials.
- The `expo` dependency is SDK 57 (`~57.0.25`). The "Mobile" section of the root AGENTS.md still says Expo 52; check the installed version before you upgrade anything.
- Keep the API contract in `@open-sunsama/api-client` and `@open-sunsama/types`. Do not call endpoints directly from screens.

## Related

- [README.md](README.md)
- [../web/AGENTS.md](../web/AGENTS.md) (the web client this app mirrors)
