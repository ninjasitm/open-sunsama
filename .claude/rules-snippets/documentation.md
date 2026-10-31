## Overview

This monorepo uses a **distributed documentation strategy** where app-specific documentation lives with each app, and only monorepo-wide concerns are documented at the root level.

## Documentation Location Rules

### CRITICAL: App-Specific vs Root Documentation

**App-specific documentation** must live in respective app folders:

- **api Features** → `apps/api/docs/features/<feature>/`
- **web Features** → `apps/web/docs/features/<feature>/`
- **api Fixes** → `apps/api/docs/fixes/`
- **web Fixes** → `apps/web/docs/fixes/`
- **Shared Package Features** → `packages/{{PACKAGE_NAME}}/docs/features/`
- **Shared Package Fixes** → `packages/{{PACKAGE_NAME}}/docs/fixes/`

**Factory features are root-scoped.** The factory pipeline runs from the monorepo root, so features it runs live at root `docs/features/<feature>/` (for example `docs/features/PROJ-1234-onboarding/`). Other features stay in their app or package folder as listed above. `<feature>` is the per-run feature folder name: `<KEY>-<slug>`, or `<slug>` when there is no ticket.

**Root `docs/` folder** is ONLY for monorepo-wide documentation:

- Architecture decisions affecting multiple apps (`docs/architecture/`)
- Deployment pipelines and infrastructure
- Cross-app integration patterns (`docs/architecture/`, one ADR per decision)
- General project overview and getting started
- Monorepo-wide tooling and configuration
- Operator and developer guides (for example `docs/RAILWAY_SETUP.md`, or a new `docs/*.md` file); onboarding lives in `README.md` and `CONTRIBUTING.md`
- Project constitution (`docs/constitution.md`, written by the constitution prompt; not created yet)

### Decision Tree: Where to Document?

```
Is this change specific to ONE app or package?
├── YES → Document in `apps/{{APP_NAME}}/docs/` or `packages/{package}/docs/`
└── NO → Does it affect multiple apps or the monorepo structure?
    ├── YES → Document in root `docs/`
    └── NO → You might be documenting the wrong thing
```

## Per-App Documentation Obligations

These rules apply to **every app** in `apps/*` and **every package** in `packages/*`. The trigger for writing docs is a code change in that app or package, not just feature-level work.

| App / Package       | Change type                                                        | Required doc action                                                                                        |
| ------------------- | ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| `apps/api`    | New or modified API route / controller                             | Update `docs/api/` endpoint spec **and** add/update the corresponding page in `apps/web/src/content/docs/` |
| `apps/api`    | New module / service                                               | Add to feature spec (`docs/features/`) and note architectural choices                                      |
| `apps/api`    | Breaking change (renamed/removed endpoint, changed response shape) | Update `docs/api/`, `apps/web/src/content/docs/`, **and** `CHANGELOG.md`                                   |
| `apps/web`    | New page / UI feature                                              | Update feature spec (`docs/features/`) and add/update page in `apps/web/src/content/docs/` if user-facing  |
| `apps/web`    | New route handler or API route                                     | Update relevant feature spec                                                                               |
| `apps/web` | Content-only edits (MDX/MDC, navigation config)                    | No extra docs required — changes here **are** the documentation update                                     |
| `packages/*`        | New or changed public API                                          | Update `docs/api/` and add/update the relevant page in `apps/web/src/content/docs/`                        |

> **Rule of thumb:** If you added or changed a controller, an API route handler, or any function that is callable from another app or external client, you must update both `docs/api/` **and** `apps/web/src/content/docs/` before committing.

## Docs App (`apps/web`) Sync Requirements

If the monorepo includes a documentation app (e.g., Docus, Nuxt Content, Docusaurus, Nextra, Starlight), the site content **must be updated as part of every commit that changes user-facing behavior**.

### When to Update the Docs App

| Trigger                       | Action in `apps/web/src/content/docs/`           |
| ----------------------------- | ------------------------------------------------ |
| New feature shipped           | Create or update the relevant documentation page |
| API endpoint added or changed | Update the API reference page                    |
| Breaking change               | Add migration guide or update affected pages     |
| Bug fix that changes behavior | Update any page that described the old behavior  |
| New shared package public API | Add or update the package reference page         |
| UI component added            | Add usage examples page if user-facing           |
| Configuration change          | Update setup / configuration pages               |

### What Does NOT Require a Docs App Update

- Internal refactors with no behavior change
- CI/CD pipeline changes (unless they affect developer setup)
- Test-only changes
- Dependency updates with no API changes
- Content edits within `apps/web` itself (these are the docs)

## App-Specific Documentation

### Feature Documentation Structure

Each app maintains its own feature documentation:

**Location:** `apps/{{APP_NAME}}/docs/features/<feature>/`

**Required Files:**

```
apps/api/docs/features/<feature>/
├── spec.md       # Functional specification
├── plan.md       # Implementation plan
└── [optional]    # Code examples, component diagrams
```

### Fix & Bug Documentation

**App-specific fixes follow a tiered structure:**

#### Decision Tree

**Before creating any fix documentation, ask:**

1. **Is this a complex fix?** (multi-file, architectural changes, affects multiple features)
   - ✅ YES → Create folder `apps/{{APP_NAME}}/docs/fixes/{{ISSUE_ID}}-{{FIX_NAME}}/` with `spec.md` + `plan.md`
   - ❌ NO → Add entry to `apps/{{APP_NAME}}/docs/fixes/{{YYYY-MM}}.md` (monthly log)

2. **Does the monthly log for this month exist?**
   - ❌ NO → Create it first using the template below
   - ✅ YES → Add your fix as a new section

**⚠️ CRITICAL: Do NOT create individual `.md` files for simple fixes!**

## Root-Level Documentation

### When to Use Root `docs/`

Create root-level documentation for:

1. **Architecture Decisions** affecting multiple apps
2. **Deployment & Infrastructure** (CI/CD, hosting, environments)
3. **Cross-App Integration** patterns and contracts
4. **Monorepo Tooling** (Turborepo, workspace management)
5. **Getting Started** guides for new developers
6. **Project Overview** and high-level architecture

### Architecture Decision Records (ADRs)

**Location:** `docs/architecture/{{NNNN}}-{{DECISION_TITLE}}.md`

**When to Create Root ADRs:**

- Choosing monorepo structure
- Selecting build system (Turborepo, Nx, etc.)
- API gateway vs direct app communication
- Shared package strategy
- Cross-app authentication approach
- Deployment platform decisions

## Changelog Management

### App-Specific Changelogs

Each app maintains its own `CHANGELOG.md`:

**Location:** `apps/{{APP_NAME}}/CHANGELOG.md`

### Root Changelog

**Location:** `CHANGELOG.md` (root)

Tracks monorepo-wide changes.

## Pre-Commit Documentation Gate

**⚠️ MANDATORY: Documentation MUST be written or updated BEFORE committing code. This is a hard requirement — no commit should land without the corresponding docs entry.**

### Pre-Commit Checklist

Before running `git commit`, verify ALL items that apply to your change:

**All changes:**

- [ ] **Features**: `docs/features/<feature>/spec.md` exists and reflects the current implementation
- [ ] **Features**: `docs/features/<feature>/plan.md` is updated if scope changed
- [ ] **Fixes**: Entry added to `apps/{{APP_NAME}}/docs/fixes/{{YYYY-MM}}.md` (or complex fix folder created)
- [ ] **Breaking changes**: `CHANGELOG.md` updated
- [ ] **Architectural decisions**: ADR created in `docs/architecture/` if a significant choice was made

**App changes (API routes, controllers, services):**

- [ ] New or modified endpoints documented in `docs/api/`
- [ ] Corresponding `apps/web/src/content/docs/` page created or updated
- [ ] Removed or renamed endpoints noted as breaking changes
- [ ] New modules/services referenced in the relevant feature spec

**Frontend app changes (pages, UI features):**

- [ ] New route handlers documented in the relevant feature spec under `docs/features/`
- [ ] Any public-facing features have a corresponding `apps/web/src/content/docs/` page created or updated

**`packages/*` changes:**

- [ ] New or changed public exports documented in `docs/api/` or the relevant feature spec
- [ ] `apps/web/src/content/docs/` updated if the change is developer-facing

If any applicable item is missing, **write the documentation first, then commit**.

## Best Practices

### Do

- ✅ Keep app-specific docs with app code
- ✅ Use root docs only for cross-cutting concerns
- ✅ Link between related docs across apps
- ✅ Maintain separate changelogs per app
- ✅ Document API contracts explicitly
- ✅ Update all affected app docs for shared changes
- ✅ Update `apps/web/src/content/docs/` with every user-facing change
- ✅ Keep docs app navigation in sync when adding new pages

### Don't

- ❌ Put app-specific features in root docs
- ❌ Duplicate documentation across apps
- ❌ Create individual fix files for simple bugs
- ❌ Forget to update API contract docs
- ❌ Mix app-specific and monorepo concerns
- ❌ Let cross-app integration docs become stale
- ❌ Commit feature code without updating the docs app
- ❌ Duplicate internal specs into the docs app verbatim — distill user-facing content instead
