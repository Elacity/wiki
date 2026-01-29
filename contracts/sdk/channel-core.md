# ChannelCore

The `ChannelCore` class provides a typed wrapper for interacting with the Elacity ChannelCore smart contract. This is the factory contract responsible for creating and managing Elacity channels.

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

const chainId = ChainId.Base;
const channelCore = ChannelCore.fromChainId(chainId, adapter);
```

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

**Note:** The factory for the specified `channelType` and `scope` must be registered before creating a channel.

## Factory Management

### Register Factory

Registers a factory contract for a specific channel type and scope combination.

```typescript
const tx = await channelCore.registerFactory(
  channelType,  // Channel type (uint8)
  scope,        // Channel scope (uint8)
  factoryAddr   // Address of the factory contract
);
```

**Returns:** `Promise<TransactionResponse>`

**Note:** Only the contract owner can register factories.

## Ownership Management

### Get Owner

Gets the current owner of the ChannelCore contract.

```typescript
const owner = await channelCore.owner();
```

**Returns:** `Promise<string>`

### Transfer Ownership

Transfers ownership of the contract to a new owner.

```typescript
const tx = await channelCore.transferOwnership(newOwnerAddress);
```

**Returns:** `Promise<TransactionResponse>`

**Note:** Only the current owner can transfer ownership.

### Renounce Ownership

Renounces ownership of the contract, making it ownerless.

```typescript
const tx = await channelCore.renounceOwnership();
```

**Returns:** `Promise<TransactionResponse>`

**Note:** This action is irreversible.

## Initialization

### Initialize

Initializes the contract (typically called during deployment).

```typescript
const tx = await channelCore.initialize();
```

**Returns:** `Promise<TransactionResponse>`

## Use Cases

ChannelCore is used for:
- **Channel Creation**: Creating new StandardChannel or MultiChannel instances
- **Factory Registration**: Registering channel factories for different types and scopes
- **Ecosystem Management**: Centralized control over channel creation permissions

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
