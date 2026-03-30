# Smart Contracts Manual Documentation

This folder contains hand-written documentation that complements generated contract references.

These pages are designed to be synced to the protocol wiki by `utils/build-contract-docs.sh`.

## Pages

- [History Digest](history-digest.md)
- [Legacy to V3 ABI Migration Guide](legacy-to-v3-abi-migration.md)
- [Current Architecture and Interactions](current-architecture.md)

## Build Integration

Run:

```sh
./utils/build-contract-docs.sh
```

This will:

1. generate fresh `forge doc` output in a temporary directory,
2. sync generated references to `.github/wiki/protocol/v3.0/references`,
3. sync these manual pages to `.github/wiki/protocol/v3.0/manual`.
