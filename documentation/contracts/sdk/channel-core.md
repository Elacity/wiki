# ChannelCore

> Compatibility wrapper: in protocol `v3.0`, prefer [`ChannelFactory`](./channel-factory.md). `ChannelCore` remains available for `v2.0` continuity and migration-safe upgrades.

The `ChannelCore` class provides a typed wrapper for interacting with the Elacity ChannelCore smart contract. This is the factory contract responsible for creating Elacity channels.

This is a **system-centric contract** — factory registration and ownership management are handled by the contract admin. Only channel creation is publicly available.

## Import

```typescript
import { ChannelCore } from '@elacity-js/contracts';
```

## Initialization

### With an explicit address

```typescript
const channelCore = new ChannelCore(contractAddress, adapter);
```

### With a network (recommended for ecosystem contracts)

ChannelCore has **one fixed deployment per supported network**. Use `fromChainId` to resolve the correct address automatically:

```typescript
import { ChainId } from '@elacity-js/common';
import { setupContracts } from '@elacity-js/contracts';

// Optional: default is '3.0'
setupContracts({ version: '3.0' });

const chainId = ChainId.Base;
const channelCore = ChannelCore.fromChainId(chainId, adapter);
```

> Version note: ecosystem address resolution follows the configured SDK version. In current config, v3 has Base + Arbitrum Sepolia entries; Elastos is available only via v2.

### Parameters

- `contractAddress` (`string`): The deployed address of the ChannelCore contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## Channel Creation

### Create Channel

Creates a new channel using the appropriate registered factory.

```typescript
const tx = await channelCore.createChannel(
  channelType,  // Channel type (uint8): 0 = Public, 1 = Private, etc.
  scope,        // Channel scope (uint8)
  name,         // Name of the channel
  tokenURI,     // Initial token URI for the channel metadata
  data          // Additional initialization data (hex string)
);
```

**Returns:** `Promise<TransactionResponse>`

**Note:** The factory for the specified `channelType` and `scope` must be registered by the admin before creating a channel.

## Ownership

### Get Owner

Gets the current owner of the ChannelCore contract.

```typescript
const owner = await channelCore.owner();
```

**Returns:** `Promise<string>`

## Use Cases

ChannelCore is used for:
- **Channel Creation**: Creating new StandardChannel or MultiChannel instances

## Example: Creating a Channel

```typescript
import { ChannelCore } from '@elacity-js/contracts';
import { ViemAdapter } from '@elacity-js/contracts-viem-adapter';

const channelCore = new ChannelCore(channelCoreAddress, adapter);

// Create a new public channel
const tx = await channelCore.createChannel(
  0,                    // Channel type: Public
  0,                    // Scope
  'My Channel',         // Channel name
  'https://...',        // Token URI
  '0x'                  // Additional data
);

await tx.wait();
console.log('Channel created!');
```
