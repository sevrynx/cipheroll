# 0002 — Key management with Supabase

- Status: accepted (design); implementation pending
- Date: 2026-10-08

## Context

Without Ente's server, we still need accounts, multi-device login and full restore
from the cloud, while keeping end-to-end encryption: no server operator (including
whoever has admin access to the Supabase project) may be able to decrypt photos.
Supabase Vault alone does not meet this, because Supabase manages the Vault root key.

## Decision

- **Supabase Auth** handles accounts. The app never sends the user's real password.
  It derives two independent values from the password with Argon2id (separate salt or
  domain-separated output):
  - an **auth secret**, sent to Supabase as the login password;
  - a **key-encryption key (KEK)** that never leaves the device.
- The app generates a random **master key** on-device and encrypts it with the KEK
  (libsodium secretbox, as Ente does). Only the **key bundle** is stored in Supabase:
  encrypted master key, nonces, KDF salt and parameters, and the recovery-key-encrypted
  copy. The table uses Row Level Security so each user can read only their own row.
- A **recovery key** (generated on-device and shown once) encrypts a second copy of the
  master key, the same idea as Ente's recovery key.
- Supabase Vault may wrap the bundle as an extra layer, but is never the only layer.
- Collection and file keys, album metadata and thumbnails are stored **encrypted in the
  user's cloud** (see ADR 0003). Supabase holds no photo metadata.

## Restore on a new device

Log in to Supabase → download key bundle → enter password → derive KEK → decrypt
master key → connect OneDrive → download encrypted metadata and decrypt.

## Open points

- Exact Argon2id parameters and how the auth secret is derived (reuse Ente's KDF
  parameters where possible).
- Whether to replace Supabase password login with SRP later.
