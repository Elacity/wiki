# CoreStorage

The `CoreStorage` class provides a typed wrapper for interacting with the Elacity CoreStorage smart contract. This is the central storage contract that maintains ecosystem-wide data including IP references, channel relationships, marketplace listings, and system configuration.

This is a **system-centric contract** — state mutations are handled internally by the smart contract or by authorised ecosystem contracts (gateways, factories). The SDK exposes read-only queries.

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
import { ChainId } from '@elacity-js/common';

const chainId = ChainId.Base;
const storage = CoreStorage.fromChainId(chainId, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the CoreStorage contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## IP References

### Get IP Reference

Gets the channel and token ID associated with a content ID.

```typescript
const [channel, tokenId] = await storage.ipReference(contentId);
```

**Returns:** `Promise<[string, bigint]>` - `[channel address, tokenId]`

## Channel and Wrapper Queries

### Get Operator

Gets the operative contract address for a channel and token ID.

```typescript
const operativeAddress = await storage.operator(channel, tokenId);
```

**Returns:** `Promise<string>`

### Get Top Level Wrappers

Gets all top-level MultiChannel wrappers for a channel.

```typescript
const wrappers = await storage.topLevelOf(channelAddress);
```

**Returns:** `Promise<string[]>` - Array of wrapper addresses

## Contract Registry

### Get Contract at Slot

Gets the contract address registered at a specific slot.

```typescript
const contractAddress = await storage.contractAt(slotBytes32);
```

**Returns:** `Promise<string>`

## Factory Registry

### Get Factory

Gets the factory address for a specific operative type.

```typescript
const factoryAddress = await storage.factories(opType);
```

**Returns:** `Promise<string>`

## Marketplace Listings

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

## Offer Queries

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

## Tax/Fee Information

### Get Tax Information

Gets current tax information.

```typescript
const [platformFee, feeRecipient] = await storage.taxInformation();
```

**Returns:** `Promise<[number, string]>` - `[platformFee, feeRecipient]`

## Acknowledgment

### Check Acknowledgment

Checks if a contract address is acknowledged.

```typescript
const isAcknowledged = await storage.acknowledged(contractAddress);
```

**Returns:** `Promise<boolean>`

## Ownership

### Get Owner

Gets the current owner of the CoreStorage contract.

```typescript
const owner = await storage.owner();
```

**Returns:** `Promise<string>`

## Use Cases

CoreStorage serves as the central read-only data layer for:
- **IP Tracking**: Querying content IDs mapped to channels and tokens
- **Operative Registry**: Looking up operative contracts for digital assets
- **Marketplace Data**: Reading listings and offers for both TradeGateway and AuthorityGateway
- **System Configuration**: Querying factories, tax information, and contract registry
- **Channel Relationships**: Querying MultiChannel wrappers and channel hierarchies
