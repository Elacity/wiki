# TradeGateway

> Compatibility wrapper: in protocol `v3.0`, prefer [`RoyaltyTradeGateway`](./royalty-trade-gateway.md). `TradeGateway` remains available for `v2.0` continuity and migration-safe upgrades.

The `TradeGateway` class provides a typed wrapper for interacting with the Elacity Trade Gateway smart contract. **This contract is dedicated to trading ERC-1155 tokens** — specifically royalty shares (`ROYALTY_SHARE` tokens from Operative contracts) and other non-access-token ERC-1155 tokens.

## Purpose

TradeGateway facilitates the marketplace for ERC-1155 token trading, allowing users to:
- List tokens for sale (`sellToken`)
- Buy listed tokens (`buyToken`)
- Remove listings (`withdrawListing`)
- Create and manage buy-offers (`createOffer`, `acceptOffer`, `cancelOffer`)

**Important:** `AccessToken(0x01)` trades are handled separately by the [`AuthorityGateway`](./authority.md). TradeGateway handles all other ERC-1155 token trades.

## Import

```typescript
import { TradeGateway } from '@elacity-js/contracts';
```

## Initialization

### With an explicit address

```typescript
const gateway = new TradeGateway(contractAddress, runner);
```

### With a network (recommended for ecosystem contracts)

TradeGateway has **one fixed deployment per supported network**. Use `fromChainId` to resolve the correct address automatically:

```typescript
import { ChainId } from '@elacity-js/common';
import { setupContracts } from '@elacity-js/contracts';

// Optional: default is '3.0'
setupContracts({ version: '3.0' });

const gateway = TradeGateway.fromChainId(ChainId.Base, runner);
```

> Version note: ecosystem address resolution follows the configured SDK version. In current config, v3 has Base + Arbitrum Sepolia entries; Elastos is available only via v2.

**Parameters:**

- `contractAddress` (`string`): The deployed address of the TradeGateway contract.
- `runner` (`IContractRunner`): An adapter instance (e.g. `EthersAdapter`, `ViemAdapter`).

---

## Listing tokens

### `sellToken(contractAddress, tokenId, quantity, pricePerToken, payToken)`

Lists tokens for sale on the marketplace. The caller must have granted ERC-1155 approval to this contract prior to calling.

```typescript
await gateway.sellToken(
  contractAddress,  // ERC-1155 contract address holding the tokens
  tokenId,          // Token ID to list
  quantity,         // Number of tokens to list
  pricePerToken,    // Price per token (smallest denomination of payToken)
  payToken          // ERC-20 payment token address (address(0) for native)
).then(tx => tx.commit());
```

### `withdrawListing(contractAddress, tokenId, quantity)`

Removes a specified quantity of tokens from the caller's active listing.

```typescript
await gateway.withdrawListing(
  contractAddress,  // ERC-1155 contract address
  tokenId,          // Token ID to delist
  quantity          // Number of tokens to remove from the listing
).then(tx => tx.commit());
```

---

## Buying tokens

### `buyToken(seller, contractAddress, tokenId, quantity, value?)`

Purchases listed tokens from a seller.

- For **native-currency** listings: `value` must cover `pricePerToken * quantity`.
- For **ERC-20** listings: the buyer must have approved this contract for the required amount.

```typescript
// ERC-20 payment
await gateway.buyToken(sellerAddress, contractAddress, tokenId, quantity).then(tx => tx.commit());

// Native currency payment
await gateway.buyToken(sellerAddress, contractAddress, tokenId, quantity, totalValue).then(tx => tx.commit());
```

---

## Offer management

### `createOfferERC20(contractAddress, tokenId, quantity, pricePerToken, payToken)`

Creates a buy-offer using an ERC-20 payment token. The caller must have pre-approved this contract for the total amount.
Only one active offer per caller per token is allowed; cancel the existing offer first.

```typescript
await gateway.createOfferERC20(
  contractAddress,
  tokenId,
  quantity,
  pricePerToken,
  payTokenAddress
).then(tx => tx.commit());
```

### `createOfferNative(contractAddress, tokenId, quantity, pricePerToken, value)`

Creates a buy-offer using native currency. `value` must be at least `pricePerToken * quantity`.
The native currency is held in escrow until the offer is accepted or cancelled.

```typescript
await gateway.createOfferNative(
  contractAddress,
  tokenId,
  quantity,
  pricePerToken,
  totalValue
).then(tx => tx.commit());
```

### `acceptOffer(from, contractAddress, tokenId, quantity)`

Accepts a pending buy-offer. The caller (seller) must own and have approved the tokens.
Platform fees are deducted before the seller receives payment.

```typescript
await gateway.acceptOffer(
  offererAddress,   // Address that created the offer
  contractAddress,
  tokenId,
  quantity          // Must be ≤ offered quantity
).then(tx => tx.commit());
```

### `cancelOffer(contractAddress, tokenId)`

Cancels the caller's pending offer. Refunds any escrowed native currency.

```typescript
await gateway.cancelOffer(contractAddress, tokenId).then(tx => tx.commit());
```

---

## Marketplace queries

### `listings(contractAddress, tokenId, seller)`

Returns the listing details for a specific seller.

```typescript
const [quantity, pricePerToken, payToken] = await gateway.listings(
  contractAddress,
  tokenId,
  sellerAddress
);
// Promise<[bigint, bigint, string]>
```

### `sellersOf(contractAddress, tokenId)`

Returns the list of addresses currently selling a given token.

```typescript
const sellers = await gateway.sellersOf(contractAddress, tokenId);
// Promise<string[]>
```

### `store()`

Returns the address of the central storage contract.

```typescript
const storageAddress = await gateway.store();
```

---

## Comparison with AuthorityGateway

| Feature | TradeGateway | AuthorityGateway |
| :--- | :--- | :--- |
| **Token type** | Any ERC-1155 (ROYALTY_SHARE, etc.) | ACCESS_TOKEN only |
| **Purpose** | Trade revenue rights & other ERC-1155s | Trade content access rights |
| **Offer system** | ✅ Yes | ❌ No |
| **Native + ERC-20** | ✅ Both | ✅ Both |
