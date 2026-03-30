# ChannelFactory

`ChannelFactory` is the canonical channel entrypoint wrapper for protocol `v3.0`.

It resolves to:
- v2: legacy `ChannelCore` ABI/address
- v3: `ChannelFactory` ABI/address

## Import

```typescript
import { ChannelFactory } from '@elacity-js/contracts';
```

## Initialization

```typescript
import { ChainId } from '@elacity-js/common';
import { setupContracts, ChannelFactory } from '@elacity-js/contracts';

setupContracts({ version: '3.0' });

const factory = ChannelFactory.fromChainId(ChainId.Base, adapter);
```

## Methods

### `createChannel(channelType, scope, name, tokenURI, data)`

```typescript
const tx = await factory.createChannel(
  1, // channelType
  1, // scope
  'My Channel',
  'ipfs://...',
  '0x'
);

await tx.commit();
```

### `factories(channelType, scope)`

```typescript
const implementationFactory = await factory.factories(1, 1);
```

### `registerFactory(channelType, scope, factoryAddress)`

Admin path for factory registration.

### `owner()`

```typescript
const owner = await factory.owner();
```

## Notes

- `ChannelCore` remains available as a compatibility alias.
- For new v3 integrations, prefer `ChannelFactory`.
