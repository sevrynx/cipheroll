# 0003 — v1 scope and storage model

- Status: accepted (scope); storage layout details pending a separate ADR
- Date: 2026-10-08

## Platform and provider

Android first (debug/independent and F-Droid flavors), OneDrive first. iOS maybe later.
Distribution: GitHub releases and F-Droid. F-Droid will flag the `NonFreeNet`
anti-feature (OneDrive, Supabase), which is acceptable.

## In v1

Backup/upload, albums, trash, deduplication, encrypted thumbnails stored in the cloud,
Ente's existing streaming file encryption, and full restore on a new device.

## Cut from v1

Sharing, collaborative albums, public links, family plans, billing and storage bonus,
referrals, Cast, push notifications, Ente email OTP/2FA/passkeys, and server-side ML
sync. On-device ML may stay if its models can be self-hosted; otherwise it is disabled.

## Storage principles (details in a later ADR)

- Everything in OneDrive is encrypted; file and folder names are random IDs.
- Metadata (collections, file keys, magic metadata, trash) is synced as encrypted,
  append-only, per-device change logs, so two devices never write the same file.
- Local SQLite is a cache rebuilt from the cloud, never the only copy.
- OneDrive access is limited to the app folder (`Files.ReadWrite.AppFolder`), and needs
  a Microsoft Entra app registration (client ID only, no secret).
