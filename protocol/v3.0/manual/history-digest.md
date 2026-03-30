# `.history` Digest

This document summarizes all files in `.history/*.md` and captures the architectural evolution from legacy to the current V3 protocol.

## 0) `0-legacy-architecture.md`

Legacy protocol baseline:

- Gateway-centric entrypoints: `AuthorityGateway`, `TradeGateway`, `ChannelCore`.
- Central hub: `CoreStorage` mixing trackers and registries (`SystemTracker`, `FactoryTracker`, `IPTracker`, `MarketplaceTracker`, `ChannelRegistry`, `ContractRegistry`, `IPMapperModule`, `FeesInformation`).
- Channel contracts (`DigitalAssetPublic`, `DigitalAssetPrivate`, `MultiChannel`) carried heavy subscription/royalty/payment stack.
- Operative types:
  - Type 1: `OperativeBuyable`
  - Type 2: `OperativeBuyableSellable`
- Payment path used `WithdrawablePaymentProcessor` with direct/deferred modes.
- Known legacy weaknesses later addressed in V3:
  - unprotected `ContractRegistry.registerAt(...)`
  - content binding path split via `IPMapperModule`
  - older access/license flow assumptions.

## 1) `1-architecture-transition.md`

Defines concrete legacy -> V3 delta via commits:

- `CoreStorage` -> `CentralStorage`.
- `ChannelCore` routing replaced by `ChannelFactory`.
- Asset orchestration moved to standalone `AssetFactory` implementing `IDigitalAssetCreator`.
- IP binding consolidated to `IPTracker` (`bindIP`) with stronger authorization.
- `ContractRegistry.registerAt(...)` hardened to `onlyOwner`.
- `AuthorityGateway.hasAccessByContentId(...)` moved to typed lookup and now supports subscription-based access checks.
- On-chain `acquireLicense(...)` flow removed from primary path; Lit-compatible off-chain model emphasized.

## 2) `2-test-porting.md`

Legacy Hardhat suite migrated to Foundry with explicit keep/adapt/drop policy.

- Added broad suite coverage for channel lifecycle, gateways, payment, fees, content access, transfer restrictions, smart accounts, and attack vectors.
- Recorded adaptation rules for V3 naming and architecture changes.
- Established function-level checklist and naming conventions for success/fuzz/revert tests.

## 3) `3-code-review.md`

Structured review and hardening backlog with implemented fixes.

Major themes:

- Reentrancy protections added on sensitive gateway/payment flows.
- CEI ordering corrected in trade/access purchase and offer acceptance paths.
- Payment safety tightened (native/erc20 mismatch handling, fee-on-transfer checks, USDT-compatible allowance update).
- Upgradeability discipline reinforced (proxy-safe init patterns, ERC-7201 consistency).
- Deployment scripts moved to manifest-driven resolution patterns.

## 4) `4-subscription-module-singleton-refactor-phase0-original-spec.md`

Original design spec for extracting subscription concerns to a singleton manager.

Key identified challenges:

- ERC-1155 token-space entanglement between channel and subscription artifacts.
- Per-channel payment processor and royalty accounting dependencies.
- Channel-scoped role/governance and token-access thresholds.

Resulting direction:

- pursue singleton manager model for subscription state and policy,
- preserve channel-facing UX where possible,
- treat approval/payment scope trade-offs explicitly.

## 5) `5-subscription-refactor-progress-summary.md`

Implementation progress for subscription isolation.

Delivered:

- `ISubscriptionManager` and `SubscriptionManager` added.
- Channel APIs preserved and delegated to manager-backed flow.
- Transparent proxy governance model for manager deployment/upgrade.
- Factory wiring and scripts updated to pass manager address.
- New dedicated manager tests added.

EIP-170 pressure noted and then partially remediated with compact action encoding and endpoint simplification.

## 6) `6-event-hub-implementation-summary.md`

Event architecture moved to centralized EventHub model.

- Added `IEventHub`, `EventHub`, `EventHubResolver`.
- Initial fallback mode evolved to mandatory resolution mode (`EventHubUnavailable` on missing hub).
- Event ownership boundaries refined:
  - EventHub keeps selected cross-cutting events (asset created, payment, token-access)
  - some trade/subscription events reverted to local ownership where appropriate.
- Factories/fixtures updated to acknowledge runtime emitters (especially payment processors).

## 7) `7-royalty-cap-protocol-shares-summary.md`

Royalty policy hardening and protocol share model.

- Introduced shared `RoyaltyModule` (`MAX_ROYALTY_SUPPLY = 1000`).
- Added protocol share cap constant (`MAX_PROTOCOL_SHARES = 250`, i.e., 25%).
- Storage API renamed toward protocol-share semantics (`protocolShares`, `assignProtocolShares`).
- Channel and operative mint flows updated for protocol share injection and cap enforcement.
- Added tests for cap invariants and protocol share configuration.

## 8) `8-erc1155-metadata-compliance-summary.md`

Metadata consistency update for ERC-1155 wallets/indexers.

- Unified subscription API to `subscribePlan(uint8 planId, bytes args)`.
- Manager/channel path now decodes args and applies per-token URI where needed.
- Explicit plan metadata update endpoint (`setPlanTokenURI`) introduced.
- Subscription signature migration propagated across tests and flows.
- Emphasis: `uri(uint256)` compliance is primary for ecosystem tooling.

## 9) `9-eip170-remediation-session.md`

Size-pressure remediation and tooling.

- `utils/preflight-check.sh` became fail-fast.
- Added `utils/contract_size_analyzer.py`.
- Reduced subscription/event-routing byte overhead.
- Post-session runtime sizes:
  - `DigitalAssetPrivate`: 24,520 B
  - `DigitalAssetPublic`: 24,396 B

## 10) `10-implementation-alignment.md`

Alignment snapshot after refactors.

- Confirms no checked-in historical storage baseline currently.
- Enumerates ERC-7201-adopted modules vs contracts still on linear storage.
- Confirms SubscriptionManager governance model is `OwnableUpgradeable` + transparent proxy (not UUPS).
- Confirms strict EventHub resolver path and failure behavior.

## 99) `99-event-watcher-plan.md`

Indexer/watcher architecture plan.

- HTTP polling (`eth_getLogs`) preferred over WebSocket for reliability/cost.
- EventHub proposed to collapse polling complexity from dynamic-contract fanout to single-address monitoring.
- Defines fallback two-tier polling architecture if hub not used.
- Establishes chain-specific deployment manifest loading for multi-chain support.

## Consolidated Timeline

1. Legacy baseline documented (`0`).
2. Core migration and security hardening (`1`).
3. Test migration and parity coverage (`2`).
4. Broad code review hardening loop (`3`).
5. Subscription singleton design then implementation (`4`, `5`).
6. Event routing centralization and boundary refinement (`6`).
7. Royalty/protocol share invariants (`7`).
8. ERC-1155 metadata + subscription ABI simplification (`8`).
9. EIP-170 byte-budget remediation (`9`).
10. Post-refactor implementation alignment and storage posture (`10`).
11. Watcher/indexing architecture planning (`99`).
