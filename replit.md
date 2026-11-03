# Dynamic Island

Native Kotlin Android app with a permission-controlled, Dynamic Island-style charging and media overlay.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Open `android-dynamic-island/` in Android Studio to build and run the native Android app; Android SDK 35 is required
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)
- Android app: Kotlin, Android Gradle Plugin, Android SDK 35

## Where things live

- `android-dynamic-island/` — standalone Android Studio project and Kotlin overlay service
- `artifacts/api-server/` — shared Express API
- `lib/api-spec/openapi.yaml` — API contract source of truth
- `lib/db/src/schema/` — database schema

## Architecture decisions

- The overlay is started only from the visible app after Android grants the special overlay permission.
- A foreground service notification provides a second, always-available stop control.
- Android's optional, broad notification access is used only for active media-session details and playback controls; the app does not inspect notification text.
- The app does not use an accessibility service or relaunch on boot.

## Product

Dynamic Island shows battery charging activity and expandable media controls in a compact overlay above other apps. Users can start it from the app and stop it from its ongoing notification.

## User preferences

- The requested Android implementation is native Kotlin.

## Gotchas

- Android overlay access is granted through system settings, not a normal runtime permission dialog.
- The Android project requires Android SDK 35 and is not part of the pnpm build.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
