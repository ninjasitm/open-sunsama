# AGENTS.md - Web (`apps/web`)

React 19 + Vite single-page app on port 3000. It serves the product UI, the marketing pages, the MDX blog, and the docs. Parent rules: [../../AGENTS.md](../../AGENTS.md) and [../../CLAUDE.md](../../CLAUDE.md).

## Commands

Run from the repository root unless noted.

- Local dev (safe): `bun run dev:local` (web on :3000, API on :3001; override with `WEB_PORT` and `API_PORT`)
- Test: `bunx turbo run test --filter=@open-sunsama/web` (Vitest, passes with no tests)
- Typecheck and lint: `bunx turbo run typecheck --filter=@open-sunsama/web` and `bunx turbo run lint --filter=@open-sunsama/web`
- Build: `bunx turbo run build --filter=@open-sunsama/web` (also generates the sitemap and prerenders blog and marketing meta)

## Stack

- TanStack Router (file routes, generated `routeTree.gen.tsx`) and TanStack Query (with a persister)
- Radix UI primitives, Tailwind CSS, `@dnd-kit` (kanban), Tiptap (rich text), lucide icons
- Data comes from `@open-sunsama/api-client`; sync uses WebSocket
- Tauri plugins are used only inside the desktop app (`useDesktop`, `@tauri-apps/*`). The web build must still run in a browser.

## Structure

- `src/routes/`: TanStack Router routes (`/app`, `/app/calendar`, `/app/focus/$taskId`, `/app/settings`, plus marketing and auth pages)
- `src/components/`: feature folders (`kanban`, `calendar`, `focus`, `tasks`, `settings`, `command-palette`, and more); shared primitives in `ui/`
- `src/hooks/`: data and UI hooks (`useIdeas`, `useCalendars`, `useAuth`, and others)
- `src/content/`: MDX blog and the docs app (`src/content/docs/`)
- `src/lib/`: client helpers, including `api.ts`
- `server.ts`: production server (`bun run start`)

## Patterns

- New UI goes in a feature folder under `src/components/`. Put reusable primitives in `src/components/ui/`.
- Use Radix primitives and Tailwind classes. Avoid global CSS outside the Tailwind setup.
- Data goes through TanStack Query hooks over `@open-sunsama/api-client`. Keep components thin.
- Target WCAG 2.1 AA: semantic HTML, keyboard paths, and focus states. The command palette (Cmd+K) and shortcuts are part of the UX.
- Design from the mobile breakpoint up.

## Gotchas

- The desktop app bundles this build (`apps/desktop` runs `../web`). Web changes reach desktop users only after a release (AGENTS.md, Desktop Releases).
- `VITE_API_URL` must be set for builds (AGENTS.md, Environment).
- Keep large marketing images out of the desktop bundle. `prepare-frontend.mjs` drops `blog-*`, `landing/`, and `og-image.png` and fails above 60 MB.

## Related

- [README.md](README.md)
- [../desktop/AGENTS.md](../desktop/AGENTS.md) (the desktop shell around this app)
- [../../packages/api-client/](../../packages/api-client/) (API client and React Query hooks)
