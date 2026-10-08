# Cipheroll — Architecture

_Living document. Decisions are in [adr/](adr/)._

## Overview

```
Flutter app (mobile/apps/photos)
 ├─ UI + services (Ente's existing structure, kept as-is — ADR 0001)
 ├─ gateways/          ← seam where Ente's server calls are replaced
 │    ├─ Supabase      : auth, encrypted key bundle (ADR 0002)
 │    └─ OneDrive      : encrypted blobs, thumbnails, metadata logs (ADR 0003)
 ├─ local SQLite       : cache, rebuilt from the cloud
 └─ crypto             : ente_crypto_api / ente_crypto_dart_adapter (libsodium) + Rust (frb)
```

## Trust model

- Supabase and OneDrive only ever see ciphertext, random IDs and the key bundle
  encrypted with a key derived from the user's password.
- Keys are generated and unwrapped on the device only.

## Upstream

`upstream` = `ente/ente`. Internal `ente_*` identifiers are kept to make merging easier.
