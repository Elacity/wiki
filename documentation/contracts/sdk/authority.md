# AuthorityGateway

The `AuthorityGateway` class provides a typed wrapper for interacting with the Elacity Authority Gateway smart contract. It handles access control checks, license validation, access token purchases, and marketplace listings.

## Import

```typescript
import { AuthorityGateway } from '@elacity-js/contracts';
```

## Initialization

```typescript
const gateway = new AuthorityGateway(contractAddress, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the AuthorityGateway contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## Access Control Methods

### Check Access

Check if an account has a valid license or access token for a specific asset.

```typescript
const hasAccess = await gateway.hasAccess(
  userAddress,      // Address to check
  channelAddress,   // Address of the channel (ledger)
  tokenId           // Token ID to check
);
```

**Returns:** `Promise<boolean>` - `true` if the user has valid access.

### Check Access by Content ID

Verify access using a simplified Content ID (KID) lookup.

```typescript
const hasAccess = await gateway.hasAccessByContentId(
  userAddress,      // Address to check
  contentId         // 128-bit Content ID (hex string)
);
```

**Returns:** `Promise<boolean>`

## Purchase Access Tokens

### Buy Access (ERC-20)

Purchase access tokens using an ERC-20 payment token.

```typescript
const tx = await gateway.buyAccessERC20(
  sellerAddress,    // Address of the seller
  channelAddress,   // Address of the channel
  tokenId,          // Token ID to buy
  quantity,         // Number of tokens
  pricePerToken,    // Price per token (in wei)
  paymentTokenAddr  // Address of the ERC-20 payment token
);
```

**Returns:** `Promise<TransactionResponse>`

### Buy Access (Native)

Purchase access tokens using native currency (ETH/ELA).

```typescript
const tx = await gateway.buyAccessNative(
  sellerAddress,    // Address of the seller
  channelAddress,   // Address of the channel
  tokenId,          // Token ID to buy
  quantity,         // Number of tokens
  pricePerToken,    // Price per token (in wei)
  value             // Total native value to send (must match quantity * price)
);
```

**Returns:** `Promise<TransactionResponse>`

## Selling Access Tokens

### Sell Access

List access tokens for sale in the marketplace.

```typescript
const tx = await gateway.sellAccess(
  channelAddress,   // Address of the channel (ledger)
  tokenId,          // Token ID
  quantity,         // Number of tokens to sell
  pricePerToken,    // Price per token (in wei)
  payToken          // Address of the payment token
);
```

**Returns:** `Promise<TransactionResponse>`

> Protocol note: internal `sellAccessOnBehalf` flows are restricted by `CoreStorage.acknowledged(msg.sender)`. Integrations should treat explicit acknowledgement as the authorization primitive, not EOA-vs-contract caller type checks.


### Withdraw Listing

Remove access tokens from the marketplace.

```typescript
const tx = await gateway.withdrawListing(
  operativeAddress, // Address of the operative contract
  tokenId,          // Token ID
  quantity          // Quantity to withdraw
);
```

**Returns:** `Promise<TransactionResponse>`

## Marketplace Queries

### Get Operative Address

Get the operative contract address for a specific channel and token ID.

```typescript
const operativeAddress = await gateway.operative(
  channelAddress,   // Address of the channel
  tokenId           // Token ID
);
```

**Returns:** `Promise<string>`

### Get Sellers

Get the list of sellers offering access tokens for a specific operative and token ID.

```typescript
const sellers = await gateway.sellersOf(
  operativeAddress, // Address of the operative contract
  tokenId           // Token ID
);
```

**Returns:** `Promise<string[]>` - Array of seller addresses

### Get Listing Information

Get detailed listing information for a specific seller.

```typescript
const listing = await gateway.listings(
  operativeAddress, // Address of the operative contract
  tokenId,          // Token ID
  sellerAddress     // Address of the seller
);
// Returns: [quantity, pricePerToken, payToken]
```

**Returns:** `Promise<[bigint, bigint, string]>` - `[quantity, pricePerToken, payToken]`

## Protocol Support

### Check Lit Protocol Support

Check if the contract supports Lit Protocol.

```typescript
const supportsLit = await gateway.supportLitProtocol();
```

**Returns:** `Promise<boolean>`
