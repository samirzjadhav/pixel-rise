# Pixel Rise

Project foundation for Pixel Rise, organised as a pnpm workspace monorepo. No application features are
implemented yet — this repo currently contains only the TypeScript toolchain, configuration, and conventions
that later work will build on.

## Requirements

- Node.js 20 or newer (developed on Node 24)
- pnpm 12 (`corepack enable` will use the version pinned in `packageManager`)

## Getting started

```bash
pnpm install
cp .env.example .env
pnpm typecheck
```

On Windows PowerShell, use `Copy-Item .env.example .env` instead of `cp`.

## Scripts

Run these from the repository root; they operate on every workspace package.

| Script | Description |
| --- | --- |
| `pnpm build` | Build all packages in dependency order (`tsc -b`) |
| `pnpm typecheck` | Type-check all packages; equivalent to `build` (see note) |
| `pnpm clean` | Remove all build output and incremental caches |

TypeScript project references require referenced projects to emit declarations, so `--noEmit` is rejected
(error TS6310). `typecheck` therefore runs the same emitting build as `build`; it exists as the conventional
name for CI.

## Environment variables

All configuration is read from environment variables. `.env.example` documents every supported key; copy it to
`.env` for local development. `.env` is git-ignored, so secrets stay out of version control.

## Layout

```
apps/
  web/                 Web front end (no framework chosen yet)
  api/                 API server (no framework chosen yet)
packages/
  config/              Shared runtime configuration
  types/               Shared domain types
  validation/          Shared input validation schemas
  database/            Database client and schema access
```

Every package keeps its source in `src/` and compiles to a git-ignored `dist/`.

### Dependency graph

`config` and `types` are the base layer and depend on nothing. `validation` depends on `types`; `database`
depends on `types` and `config`. `api` consumes all four packages, and `web` consumes `config`, `types`, and
`validation`. Dependencies point in one direction only — packages never import from apps.

## Conventions

- TypeScript with `strict` mode plus extra safety flags; no implicit `any`, no unused locals.
- ES modules only (`"type": "module"`); use extensionful relative imports, e.g. `./config.js`.
- Shared compiler options live in `tsconfig.base.json`; each package extends it and declares its internal
  dependencies under `references` so builds stay incremental and correctly ordered.
- Internal packages are named `@pixel-rises/*` and linked with the `workspace:*` protocol, so they always
  resolve to local source rather than the npm registry.
- A package must declare a dependency before importing it. pnpm's isolated `node_modules` means an undeclared
  import fails to resolve rather than working by accident.
