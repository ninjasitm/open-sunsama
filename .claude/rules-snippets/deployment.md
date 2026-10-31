## Apps

| App           | Platform                                                  | Command / source                                             |
| ------------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| `api`         | Railway service built from `Dockerfile.api` (`/health`)   | Railway; see `railway.toml`                                  |
| `web`         | Railway service built from `Dockerfile.web`               | Railway; see `railway.toml`                                  |
| `desktop`     | Tauri v2 installers via the Desktop Release workflow      | `bun run release` (see AGENTS.md, Desktop Releases)          |
| `mobile`      | Tauri v2 iOS and Android builds                           | `bun run build:mobile:ios` / `bun run build:mobile:android`  |
| `expo-mobile` | Expo / EAS                                                | `bun run build:ios` or `bun run build:android` in `apps/expo-mobile` |

## Environment Variables

### Root Level

The root `.env.local` holds test credentials. It is gitignored; never commit it. `.env.example` lists the optional variables for self-hosting.

### Per-App

```bash
# apps/api/.env (local only; see AGENTS.md before starting a local API)
DATABASE_URL=postgresql://...
JWT_SECRET=...
PORT=3001
CORS_ORIGIN=http://localhost:3000

# apps/web/.env
VITE_API_URL=http://localhost:3001
```

## CI/CD

`.github/workflows/ci.yml` runs the checks (install with a frozen lockfile, typecheck, lint, unit tests, build, migrations against the schema, desktop bundle size) and the end-to-end job. `.github/workflows/desktop-release.yml` builds the desktop apps.

## Pre-Deployment Checklist

- [ ] All tests pass across workspace
- [ ] Linting passes
- [ ] Build completes for all affected apps
- [ ] Environment variables configured per app
- [ ] Database migrations applied (if applicable)
- [ ] Shared package changes tested in all consuming apps
