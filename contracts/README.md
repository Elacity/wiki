# Contracts Documentation Versions

This section is versioned.

## Available Versions

- [v3.0 (current)](v3.0/README.md)
- [v2.0 (legacy)](v2.0/README.md)

## Notes

- `v3.0` tracks the current `v3-drm-protocol` architecture (`CentralStorage`, `ChannelFactory`, `RoyaltyTradeGateway`, `SubscriptionManager`, `EventHub`).
- `v2.0` preserves the previous documentation snapshot for older integrations.

## Build Pipeline

Regenerate only the current docs version (`v3.0`):

```sh
./utils/build-contract-docs.sh
```
