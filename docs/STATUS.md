# Cipheroll — Status

_Read this first each session. Last updated: 2026-10-08._

## Current state

- Fork of `ente/ente` cloned; `upstream` remote added.
- Baseline debug APK of the **unmodified** app builds on the dev host
  (see README → Build). Not yet run on a device.
- Decisions recorded: ADR 0001 (fork/rebrand), 0002 (keys/Supabase), 0003 (v1 scope).

## Next steps

1. Rebrand to Cipheroll (visible branding, app ID `app.cipheroll`, disconnect Ente services).
2. v1 scope cuts (ADR 0003).
3. ADR 0004: OneDrive layout and metadata sync format.
4. Supabase schema + key bundle (ADR 0002).
5. OneDrive gateway replacing Ente's `gateways/` + upload/download paths.

## Needed from Khan (later)

- Supabase project URL + anon key, and a personal access token (via secrets prompt).
- Microsoft Entra app registration client ID (steps will be provided).
- A throwaway OneDrive account for testing; a way to test on a device.
- Release signing key: generate here or Khan creates it.

## Assumptions

- Internal `ente_*` identifiers stay (ADR 0001).
- Ente's architecture is kept as-is; efficiency over refactoring (ADR 0001).

## Known issues

- Flutter warns Gradle 8.14.3 / AGP 8.12.1 / Kotlin 2.2.20 will soon be unsupported.
- Host needs a user-local `unzip` (rive_native build) — see README.
