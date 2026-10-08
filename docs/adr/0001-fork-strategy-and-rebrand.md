# 0001 — Fork strategy and rebrand to Cipheroll

- Status: accepted
- Date: 2026-10-08

## Context

Cipheroll is a fork of the Ente monorepo (AGPL-3.0). Ente's server ("Museum") and
storage are replaced with Supabase (auth + encrypted key bundle) and the user's own
cloud storage (OneDrive first). Cipheroll will be published as open source on
GitHub and F-Droid. The original name "Crypted Gallery" was already used by an
active Play Store app (`fr.gregoryd.cryptedgallery`), so the app is named Cipheroll.

## Decision

1. **Keep everything we legally can.** Ente's code is AGPL-3.0, so we may reuse
   and modify it as long as Cipheroll stays AGPL-3.0, publishes full source, and
   keeps Ente's copyright and license notices. Attribution lives in `NOTICE.md`,
   the README, and the in-app About screen.
2. **Remove what we may not or should not keep:**
   - User-visible branding: the "Ente" name, logo, the "Ducky" mascot artwork,
     store badges, Ente app/store links and marketing copy.
   - Anything that talks to Ente-operated services: API servers, Sentry reporter,
     update checks, payments, cast, public albums, family, model CDN (`models.ente.*`),
     and so on. Pointing a fork at Ente's infrastructure would be abuse of their service.
   - Android app ID `io.ente.photos` → `app.cipheroll` (flavors keep their suffixes).
3. **Keep internal identifiers** (`ente_*` packages, `EnteFile`, Rust crate names).
   Renaming them touches thousands of files and would make upstream security fixes
   very hard to merge. They are not branding.
4. **Track upstream** as the `upstream` git remote (`ente/ente`). Changes stay
   isolated where cheap; we cherry-pick security fixes.
5. **Architecture:** keep Ente's existing structure (singleton services, `gateways/`).
   Don't refactor it into the default Riverpod/Clean Architecture playbook; Khan's
   priority is efficiency. New modules follow the existing style unless a cleaner
   seam is equally cheap.

## Consequences

- AGPL obligations apply to every release (source link in app and release notes).
- Merges from upstream will conflict on rebranded strings/assets; resolve by keeping ours.
- On-device ML models must be self-hosted (e.g. GitHub release assets) if kept.
