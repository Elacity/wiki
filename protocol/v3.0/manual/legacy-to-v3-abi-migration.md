# Legacy to V3 ABI Migration Guide

This guide is for integrators migrating ABI consumers from the legacy ecosystem to current V3 contracts.

## 1. Major ABI Surface Changes

### Core entrypoint contract changes

- `CoreStorage` -> `CentralStorage`
- `ChannelCore` -> `ChannelFactory`
- `TradeGateway` -> `RoyaltyTradeGateway`
- legacy `DigitalAssetCreator` implementation -> `AssetFactory` as explicit orchestrator (`IDigitalAssetCreator`)

### Removed / deprecated paths

- Legacy on-chain license acquisition path (`acquireLicense`) is no longer the primary model.
- `IPMapperModule`-based external assumptions should be replaced by `IPTracker`-backed lookups and binding paths.

### Subscription ABI evolution

- Old style: `subscribePlan(planId, recurring)`
- Current style: `subscribePlan(uint8 planId, bytes args)`
- Current baseline behavior: recurring semantics are disabled (`false`) and extensible args carry metadata context.
- SDK-facing usage: call subscription methods from `StandardChannel` / `MultiChannel` wrappers directly (there is no standalone `SubscriptionModule` SDK wrapper).

### Naming normalization

- `bindIp(...)` -> `bindIP(...)`

## 2. Practical ABI Migration Steps

1. Refresh contract address sources.
- Load addresses from `deployments/<chainid>.json`.
- Stop relying on hardcoded legacy slot assumptions.

2. Regenerate ABIs from current build artifacts.
- Use `forge build` then consume ABIs from `artifacts/`.
- Ensure your generated client bindings match Solidity `0.8.29` outputs.

3. Replace renamed contracts and methods in client code.
- gateway references (`TradeGateway` -> `RoyaltyTradeGateway`)
- routing references (`ChannelCore` -> `ChannelFactory`)
- storage references (`CoreStorage` -> `CentralStorage`)
- subscription call signatures (`subscribePlan` args shape)

4. Update event decoding subscriptions.
- Include EventHub-based topics where configured.
- Keep support for locally-owned events where the protocol intentionally retained local emission.

5. Revalidate permission and role assumptions.
- `registerAt` and `bindIP` are now hardened paths.
- Do not assume public write behavior from legacy contracts.

6. Verify payment path behavior per listing type.
- native and ERC-20 purchase paths are stricter in V3.
- treat mismatched payment mode as explicit errors, not silent acceptance.

7. Re-run integration tests against V3 fixtures.
- cover channel creation, minting, access purchase, royalty-share trading, subscription, and rewards withdrawal.

## 3. Legacy -> V3 ABI Mapping Table

| Legacy | V3 | Integrator Action |
|---|---|---|
| `CoreStorage` | `CentralStorage` | swap storage ABI + address source |
| `ChannelCore.createChannel` | `ChannelFactory.createChannel` | update channel deployment entrypoint |
| `TradeGateway` market ABI | `RoyaltyTradeGateway` market ABI | switch client contract binding |
| `IPMapperModule.bindIp` assumptions | `IPTracker.bindIP` + tracker resolution | update binding call paths and naming |
| `subscribePlan(planId, recurring)` | `subscribePlan(planId, args)` | update call encoding and frontend params |
| split local events only model | EventHub + local event mix | update decoder routing strategy |

## 4. Suggested ABI-Compatibility Test Checklist

- `createChannel` succeeds for each channel type/scope.
- `mint` flows still emit and index expected asset creation metadata.
- `sellAccess` + `buyAccess` works for native and ERC-20 paths.
- `sellToken` + `buyToken` + offer lifecycle works in `RoyaltyTradeGateway`.
- `subscribePlan(planId, args)` works with empty args and encoded URI args.
- `hasAccessByContentId` resolves both operative and subscription-based access.
- rewards withdraw functions work for channel and operative recipients.

## 5. Migration Safety Notes

- Keep ABI versions and deployment manifests chain-specific.
- Prefer typed interface calls over low-level encoded call paths.
- Treat EventHub as required infrastructure in environments configured for mandatory routing.
- Maintain regression tests for all signature changes before releasing SDK updates.

## 6. SDK integration notes (v3)

```typescript
// Standard channel
await channel.subscribePlan(planId); // empty args -> 0x
await channel.subscribePlan(planId, { tokenURI: 'ipfs://.../subscription-token.json' }, value);

// Multi-channel
await multi.subscribePlan(planId);
await multi.subscribePlan(planId, { tokenURI: 'ipfs://.../subscription-token.json' }, value);
```
