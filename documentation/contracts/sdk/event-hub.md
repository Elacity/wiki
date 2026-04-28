# EventHub

`EventHub` is a v3-only ecosystem contract used to centralize protocol event emission for selected event classes.

## Import

```typescript
import { EventHub } from '@elacity-js/contracts';
```

## Initialization

```typescript
import { ChainId } from '@elacity-js/common';
import { setupContracts, EventHub } from '@elacity-js/contracts';

setupContracts({ version: '3.0' });

const eventHub = EventHub.fromChainId(ChainId.Base, adapter);
```

> `EventHub.fromChainId` is available only when the configured version has `EVENT_HUB` address entries (currently v3).

## Management Methods

```typescript
const owner = await eventHub.owner();
const tracker = await eventHub.systemTracker();
await eventHub.setSystemTracker(newSystemTracker);
```

## Emission Methods

```typescript
await eventHub.emitAssetCreated(authorityGateway, channel, tokenId, cid, opType, operative);
await eventHub.emitPaymentCommitted(contractAddress, payToken, from, to, amount);
await eventHub.emitPaymentLog(contractAddress, payToken, from, amount, paymentProcessor);
await eventHub.emitRewardsIncremented(contractAddress, to, amount, payToken, by);
await eventHub.emitRewardsWithdrawn(contractAddress, to, amount, payToken);
await eventHub.emitTokenAccessRegistered(contractAddress, tokenAddress, threshold);
await eventHub.emitTokenAccessRemoved(contractAddress, tokenAddress);
await eventHub.emitAtomicNativeTransfer(from, to, amount);
```

## Notes

- Event emission methods are generally intended for acknowledged protocol contracts.
- External integrators commonly read EventHub events rather than emit them.
