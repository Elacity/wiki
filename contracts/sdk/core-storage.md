# CoreStorage

The `CoreStorage` class provides a typed wrapper for interacting with the Elacity CoreStorage smart contract. This is the central storage contract that maintains ecosystem-wide data including IP bindings, channel relationships, marketplace listings, and system configuration.

## Import

```typescript
import { CoreStorage } from '@elacity-js/contracts';
```

## Initialization

### With an explicit address

```typescript
const storage = new CoreStorage(contractAddress, adapter);
```

### With a network (recommended for ecosystem contracts)

CoreStorage has **one fixed deployment per supported network**. Use `fromChainId` to resolve the correct address automatically:

```typescript
import { ChainId } from '@elacity-js/core';

const chainId = ChainId.Base;
const storage = CoreStorage.fromChainId(chainId, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the CoreStorage contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## IP (Intellectual Property) Management

### Bind IP

Binds an Intellectual Property (content ID) to a channel and token ID.

```typescript
const tx = await storage.bindIP(
  contentId,  // 128-bit Content ID (hex string, bytes16)
  channel,    // Address of the channel
  tokenId     // Token ID
);
```

**Returns:** `Promise<TransactionResponse>`

### Get IP Reference

Gets the channel and token ID associated with a content ID.

```typescript
const [channel, tokenId] = await storage.ipReference(contentId);
```

**Returns:** `Promise<[string, bigint]>` - `[channel address, tokenId]`

## Digital Asset Registration

### Register Digital Asset

Registers an operative contract for a channel and token ID.

```typescript
const tx = await storage.registerDigitalAsset(
  channel,  // Address of the channel
  tokenId,  // Token ID
  op        // Address of the operative contract
);
```

**Returns:** `Promise<TransactionResponse>`

### Get Operator

Gets the operative contract address for a channel and token ID.

```typescript
const operativeAddress = await storage.operator(channel, tokenId);
```

**Returns:** `Promise<string>`

## Channel and Wrapper Management

### Add Wrapper

Adds a MultiChannel wrapper for a channel.

```typescript
const tx = await storage.addWrapper(
  channel,  // Address of the channel
  wrapper   // Address of the MultiChannel wrapper
);
```

**Returns:** `Promise<TransactionResponse>`

### Get Top Level Wrappers

Gets all top-level MultiChannel wrappers for a channel.

```typescript
const wrappers = await storage.topLevelOf(channelAddress);
```

**Returns:** `Promise<string[]>` - Array of wrapper addresses

## Contract Registry

### Register Contract (String Slot)

Registers a contract address at a specific string slot.

```typescript
const tx = await storage.registerAt(
  slotStr,  // Slot identifier as string
  value     // Contract address to register
);
```

**Returns:** `Promise<TransactionResponse>`

### Register Contract (Bytes32 Slot)

Registers a contract address at a specific bytes32 slot.

```typescript
const tx = await storage.registerAtBytes32(
  slot,   // Slot identifier as bytes32 (hex string)
  value   // Contract address to register
);
```

**Returns:** `Promise<TransactionResponse>`

### Get Contract at Slot

Gets the contract address registered at a specific slot.

```typescript
const contractAddress = await storage.contractAt(slotBytes32);
```

**Returns:** `Promise<string>`

## Factory Management

### Get Factory

Gets the factory address for a specific operative type.

```typescript
const factoryAddress = await storage.factories(opType);
```

**Returns:** `Promise<string>`

### Set Operative Factory

Sets the operative factory for a specific operative type.

```typescript
const tx = await storage.setOperativeFactory(
  opType,      // Operative type (uint16)
  factoryAddr  // Address of the factory contract
);
```

**Returns:** `Promise<TransactionResponse>`

## Marketplace Listings

### Set Listing

Creates or updates a listing for tokens.

```typescript
const tx = await storage.setListing(
  op,           // Address of the operative contract
  tokenId,      // Token ID
  owner,        // Address of the owner/seller
  quantity,     // Quantity to list
  pricePerToken, // Price per token (in wei)
  payToken      // Address of the payment token
);
```

**Returns:** `Promise<TransactionResponse>`

### Get Listing

Gets listing information for a specific seller.

```typescript
const [quantity, pricePerToken, payToken] = await storage.getListing(
  operativeAddress,
  tokenId,
  sellerAddress
);
```

**Returns:** `Promise<[bigint, bigint, string]>` - `[quantity, pricePerToken, payToken]`

### Get Listing (Mapping Access)

Alternative method to get listing information.

```typescript
const [quantity, pricePerToken, payToken] = await storage.listings(
  operativeAddress,
  tokenId,
  sellerAddress
);
```

**Returns:** `Promise<[bigint, bigint, string]>`

### Get Sellers

Gets the list of sellers offering tokens for a specific operative and token ID.

```typescript
const sellers = await storage.sellersOf(operativeAddress, tokenId);
```

**Returns:** `Promise<string[]>` - Array of seller addresses

## Offer Management

### Set Offer

Creates or updates an offer for tokens.

```typescript
const tx = await storage.setOffer(
  op,           // Address of the operative contract
  tokenId,      // Token ID
  from,         // Address of the offer creator
  quantity,     // Quantity desired
  pricePerToken, // Price per token (in wei)
  payToken      // Address of the payment token
);
```

**Returns:** `Promise<TransactionResponse>`

### Get Offer

Gets offer information for a specific offer creator.

```typescript
const [quantity, pricePerToken, payToken] = await storage.getOffer(
  operativeAddress,
  tokenId,
  offerCreatorAddress
);
```

**Returns:** `Promise<[bigint, bigint, string]>` - `[quantity, pricePerToken, payToken]`

### Get Offer (Mapping Access)

Alternative method to get offer information.

```typescript
const [quantity, pricePerToken, payToken] = await storage.offers(
  operativeAddress,
  tokenId,
  offerCreatorAddress
);
```

**Returns:** `Promise<[bigint, bigint, string]>`

### Get Offerers

Gets the list of offer creators for a specific operative and token ID.

```typescript
const offerers = await storage.offerersOf(operativeAddress, tokenId);
```

**Returns:** `Promise<string[]>` - Array of offer creator addresses

## Tax/Fee Configuration

### Set Tax Information

Sets platform fee and fee recipient address.

```typescript
const tx = await storage.setTaxInformation(
  platformFee,  // Platform fee percentage (uint16, e.g., 250 = 2.5%)
  feeRecipient  // Address to receive fees
);
```

**Returns:** `Promise<TransactionResponse>`

**Note:** Only the contract owner can set tax information.

### Get Tax Information

Gets current tax information.

```typescript
const [platformFee, feeRecipient] = await storage.taxInformation();
```

**Returns:** `Promise<[number, string]>` - `[platformFee, feeRecipient]`

## Acknowledgment System

### Acknowledge Contract

Marks a contract address as acknowledged.

```typescript
const tx = await storage.ack(contractAddress);
```

**Returns:** `Promise<TransactionResponse>`

### Unacknowledge Contract

Removes acknowledgment from a contract address.

```typescript
const tx = await storage.unAck(contractAddress);
```

**Returns:** `Promise<TransactionResponse>`

### Check Acknowledgment

Checks if a contract address is acknowledged.

```typescript
const isAcknowledged = await storage.acknowledged(contractAddress);
```

**Returns:** `Promise<boolean>`

## Ownership Management

### Get Owner

Gets the current owner of the CoreStorage contract.

```typescript
const owner = await storage.owner();
```

**Returns:** `Promise<string>`

### Transfer Ownership

Transfers ownership of the contract to a new owner.

```typescript
const tx = await storage.transferOwnership(newOwnerAddress);
```

**Returns:** `Promise<TransactionResponse>`

### Renounce Ownership

Renounces ownership of the contract.

```typescript
const tx = await storage.renounceOwnership();
```

**Returns:** `Promise<TransactionResponse>`

## Initialization

### Initialize

Initializes the contract with an initial owner.

```typescript
const tx = await storage.initialize(initialOwnerAddress);
```

**Returns:** `Promise<TransactionResponse>`

## Use Cases

CoreStorage serves as the central data layer for:
- **IP Tracking**: Binding content IDs to channels and tokens
- **Operative Registry**: Tracking operative contracts for digital assets
- **Marketplace Data**: Storing listings and offers for both TradeGateway and AuthorityGateway
- **System Configuration**: Managing factories, tax information, and contract registry
- **Channel Relationships**: Tracking MultiChannel wrappers and channel hierarchies

## Example: Registering a Digital Asset

```typescript
import { CoreStorage } from '@elacity-js/contracts';
import { ViemAdapter } from '@elacity-js/contracts-viem-adapter';

const storage = new CoreStorage(coreStorageAddress, adapter);

// Register an operative for a channel token
const tx = await storage.registerDigitalAsset(
  channelAddress,
  tokenId,
  operativeAddress
);

await tx.wait();
console.log('Digital asset registered!');
```

## Example: Binding IP

```typescript
// Bind a content ID to a channel and token
const tx = await storage.bindIP(
  contentId,      // 128-bit Content ID
  channelAddress,
  tokenId
);

await tx.wait();
console.log('IP bound successfully!');

// Later, retrieve the IP reference
const [channel, tokenId] = await storage.ipReference(contentId);
console.log(`IP is bound to channel ${channel}, token ${tokenId}`);
```
