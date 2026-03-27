# Contracts - Smart Contract SDK

The `@elacity-js/contracts` package provides wrappers for the Elacity V3 smart contract ecosystem.

## What's Inside

### Getting Started

- [Design Snapshot](getting-started/design-proposal.md)
- [Ecosystem Overview](getting-started/ecosystem-overview.md)
- [Security Analysis](getting-started/security-analysis.md)
- [SDK Installation](sdk/installation.md)

### Migration & Architecture Manuals

- [Manual Docs Index](manual/README.md)
- [History Digest](manual/history-digest.md)
- [Legacy to V3 ABI Migration Guide](manual/legacy-to-v3-abi-migration.md)
- [Current Architecture](manual/current-architecture.md)

### Contract References

- [Contract API Reference](references/README.md)
- [Contract Index](references/SUMMARY.md)

### SDK Guides

- [Authority Gateway](sdk/authority.md)
- [Royalty Trade Gateway](sdk/royalty-trade-gateway.md)
- [Channel Factory](sdk/channel-factory.md)
- [Central Storage](sdk/central-storage.md)
- [Channel Wrappers](sdk/channel.md)
- [MultiChannel](sdk/multi-channel.md)
- [Operatives](sdk/operative.md)
- [Transactions](sdk/transactions.md)
- [Universal Account Executor](sdk/universal-account-executor.md)

## Current V3 Highlights

- Gateway split:
  - `AuthorityGateway` for access-token market and access checks
  - `RoyaltyTradeGateway` for non-access-token ERC-1155 trading
- Storage hub: `CentralStorage`
- Channel routing: `ChannelFactory`
- Asset orchestration: `AssetFactory`
- Subscription backend: `SubscriptionManager`
- Event aggregation (selected classes): `EventHub`

## Regenerating Docs

Run:

```sh
./utils/build-contract-docs.sh
```

This refreshes:

- `.github/wiki/contracts/references` from `forge doc` (with solidity-docgen-like formatting)
- `.github/wiki/contracts/manual` from `docs/contracts-manual`
