# Cipheroll

End-to-end encrypted photo backup to cloud storage you already own.

Cipheroll encrypts your photos and videos on your device and stores them in your own
cloud account (OneDrive first). Your account and an encrypted copy of your keys live
on a Supabase backend; nobody but you can decrypt your photos.

> **Status:** early development. Not ready for use. See [docs/STATUS.md](docs/STATUS.md).

Cipheroll is a fork of [Ente](https://github.com/ente/ente) and is not affiliated with
or endorsed by Ente. See [NOTICE.md](NOTICE.md).

## Repository layout

This is a fork of the Ente monorepo. Cipheroll currently uses:

- `mobile/apps/photos/` — the Flutter app
- `mobile/packages/` — shared Dart packages
- `rust/` — Rust core, bridged to Dart with flutter_rust_bridge

Other upstream directories (`server/`, `web/`, `desktop/`, Auth and Locker apps) are
kept for easier upstream merges but are not part of Cipheroll.

## Build (Android)

Requirements: Flutter 3.47.2, JDK 17, Android SDK 36, NDK 28.2.13676358, Rust stable
with Android targets, and `unzip` on `PATH`.

```sh
cd mobile/apps/photos
flutter pub get --enforce-lockfile
(cd ../../../rust && cargo codegen frb photos)
flutter build apk --debug --flavor independent \
  --target-platform android-arm64 --dart-define=cronetHttpNoPlay=true
```

## Documentation

- [Status and next steps](docs/STATUS.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Decision records](docs/adr/)
- [Changelog](CHANGELOG.md)

## License

GNU AGPL-3.0, same as upstream. See [LICENSE](LICENSE) and [NOTICE.md](NOTICE.md).
