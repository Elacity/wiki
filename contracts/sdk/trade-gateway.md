# TradeGateway

The `TradeGateway` class provides a typed wrapper for interacting with the Elacity Trade Gateway smart contract. **This contract is dedicated to trading royalty tokens** (specifically `ROYALTY_SHARE` tokens from Operative contracts).

## Purpose

TradeGateway facilitates the marketplace for royalty share trading, allowing users to:
- Buy and sell royalty shares from Operative contracts
- Create and manage offers for royalty tokens
- Trade future revenue rights associated with digital assets

**Important:** This gateway is specifically for **royalty token trading**, not access tokens. For access token trading, see [AuthorityGateway](./authority.md).

## Import

```typescript
import { TradeGateway } from '@elacity-js/contracts';
```

## Initialization

### With an explicit address

```typescript
const gateway = new TradeGateway(contractAddress, adapter);
```

### With a network (recommended for ecosystem contracts)

TradeGateway has **one fixed deployment per supported network**. Use `fromChainId` to resolve the correct address automatically:

```typescript
import { ChainId } from '@elacity-js/common';

const chainId = ChainId.Base;
const gateway = TradeGateway.fromChainId(chainId, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the TradeGateway contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## Listing Royalty Tokens

### Sell Token

List royalty tokens (ROYALTY_SHARE) for sale in the marketplace.

```typescript
const tx = await gateway.sellToken(
  contractAddress,  // Address of the token contract
  tokenId,          // Token ID
  quantity,         // Quantity to sell
  pricePerToken,    // Price per token (in wei)
  payToken          // Address of the payment token
);
```

**Returns:** `Promise<TransactionResponse>`

### Withdraw Listing

Remove royalty tokens from the marketplace.

```typescript
const tx = await gateway.withdrawListing(
  operativeAddress, // Address of the operative contract
  tokenId,          // Token ID
  quantity          // Quantity to withdraw
);
```

**Returns:** `Promise<TransactionResponse>`

## Buying Royalty Tokens

### Buy Token

Purchase royalty tokens from a seller in the marketplace.

```typescript
const tx = await gateway.buyToken(
  sellerAddress,    // Address of the seller
  contractAddress,  // Address of the token contract
  tokenId,          // Token ID
  quantity,         // Quantity to buy
  value             // Optional: native value to send (for native payment)
);
```

**Returns:** `Promise<TransactionResponse>`

## Offer Management

### Create Offer (ERC-20)

Create an offer for royalty tokens using an ERC-20 payment token.

```typescript
const tx = await gateway.createOfferERC20(
  contractAddress,  // Address of the token contract
  tokenId,          // Token ID
  quantity,         // Quantity desired
  pricePerToken,    // Price per token (in wei)
  payToken          // Address of the ERC-20 payment token
);
```

**Returns:** `Promise<TransactionResponse>`

### Create Offer (Native)

Create an offer for royalty tokens using native currency.

```typescript
const tx = await gateway.createOfferNative(
  contractAddress,  // Address of the token contract
  tokenId,          // Token ID
  quantity,         // Quantity desired
  pricePerToken,    // Price per token (in wei)
  value             // Total native value to send (must match quantity * price)
);
```

**Returns:** `Promise<TransactionResponse>`

### Cancel Offer

Cancel an existing offer.

```typescript
const tx = await gateway.cancelOffer(
  contractAddress,  // Address of the token contract
  tokenId           // Token ID
);
```

**Returns:** `Promise<TransactionResponse>`

### Accept Offer

Accept an offer as a seller.

```typescript
const tx = await gateway.acceptOffer(
  fromAddress,      // Address of the offer creator
  contractAddress,  // Address of the token contract
  tokenId,          // Token ID
  quantity          // Quantity to accept
);
```

**Returns:** `Promise<TransactionResponse>`

## Marketplace Queries

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

### Get Sellers

Get the list of sellers offering royalty tokens for a specific operative and token ID.

```typescript
const sellers = await gateway.sellersOf(
  operativeAddress, // Address of the operative contract
  tokenId           // Token ID
);
```

**Returns:** `Promise<string[]>` - Array of seller addresses

### Get Storage Contract

Get the storage contract address used by the TradeGateway.

```typescript
const storageAddress = await gateway.store();
```

**Returns:** `Promise<string>`

## Understanding Royalty Tokens

Royalty tokens (`ROYALTY_SHARE`) represent fractional ownership of future revenue from digital assets. When you purchase royalty tokens through TradeGateway:

- You acquire a percentage of future revenue (typically measured in 1/1000th units)
- Revenue is distributed proportionally to all royalty token holders
- Royalty tokens can be traded freely on the marketplace
- Each Operative contract manages royalty distribution for its associated digital asset

**Example:** If an Operative has 1000 total royalty shares and you own 100 shares, you own 10% of future revenue from that asset.

## Comparison with AuthorityGateway

| Feature | TradeGateway | AuthorityGateway |
| :--- | :--- | :--- |
| **Token Type** | ROYALTY_SHARE tokens | ACCESS_TOKEN tokens |
| **Purpose** | Trade future revenue rights | Trade content access rights |
| **Token Source** | Operative contracts | Operative contracts |
| **Use Case** | Investment in creator revenue | Purchase content access |
