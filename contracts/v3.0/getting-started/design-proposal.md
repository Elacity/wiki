# Elacity V3 Design Snapshot

This document captures the current design direction reflected in the repository, replacing older pre-V3 proposal assumptions.

## Design goals

- Keep a modular ERC-1155-centered ecosystem for DRM and revenue sharing.
- Separate marketplace responsibilities:
  - `AuthorityGateway` for access tokens
  - `RoyaltyTradeGateway` for non-access-token trade flows
- Maintain deterministic deployment wiring through chain manifests (`deployments/<chainid>.json`).
- Support channel subscription and content ownership access in a single access model.
- Keep indexing/event consumption practical via EventHub-centric routing.

## Core design decisions

### 1) Storage and registration hardening

- Use `CentralStorage` as the hub.
- Keep sensitive writes owner/role/whitelist constrained.
- Consolidate content binding and operative mapping in tracker modules.

### 2) Explicit orchestration contracts

- `ChannelFactory` handles channel routing/creation.
- `AssetFactory` handles operative creation and post-mint wiring.

### 3) Subscription model

- `SubscriptionManager` owns channel-scoped subscription state.
- Channels preserve user-facing subscription entrypoints as delegating facades.
- Current call shape: `subscribePlan(uint8 planId, bytes args)`.

### 4) Event model

- EventHub is first-class infrastructure for selected event classes.
- Resolver enforces configuration availability in mandatory-routing paths.

### 5) Royalty and protocol-share policy

- Royalty cap invariant is centralized.
- Protocol shares are capped and injected through configured paths.

### 6) Bytecode budget discipline

- EIP-170 margin is treated as an active constraint.
- Preflight/analysis tooling is part of the design process.

## Operational constraints

- Foundry-first toolchain (`forge`, `cast`, `anvil`).
- Upgradeability-safe coding patterns for proxy-backed contracts.
- Manifest-driven deployment scripts (avoid env-derived protocol address wiring).

## Non-goals (current baseline)

- Legacy on-chain `acquireLicense` as primary DRM flow.
- Hardhat-centric deployment/testing/docs workflows.
- Legacy `CoreStorage`/`ChannelCore`/`TradeGateway` naming surface.

## Recommended documentation path

1. [Ecosystem Overview](ecosystem-overview.md)
2. [ABI Migration Guide](../manual/legacy-to-v3-abi-migration.md)
3. [Current Architecture](../manual/current-architecture.md)
4. [Contract References](../references/README.md)
