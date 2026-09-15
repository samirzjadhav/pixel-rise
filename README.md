# Pixel Rise

Project foundation for Pixel Rise. No application features are implemented yet — this repo currently contains
only the TypeScript toolchain, configuration, and conventions that later work will build on.

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

| Script | Description |
| --- | --- |
| `pnpm typecheck` | Type-check the project without emitting files |
| `pnpm build` | Compile `src/` to `dist/` |
| `pnpm clean` | Remove the `dist/` directory |

## Environment variables

All configuration is read from environment variables. `.env.example` documents every supported key; copy it to
`.env` for local development. `.env` is git-ignored, so secrets stay out of version control.

## Layout

```
src/        Application source (TypeScript, ES modules)
dist/       Compiled output (generated, git-ignored)
```

## Conventions

- TypeScript with `strict` mode plus extra safety flags; no implicit `any`, no unused locals.
- ES modules only (`"type": "module"`); use extensionful relative imports, e.g. `./config.js`.
