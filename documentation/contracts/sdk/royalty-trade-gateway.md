# RoyaltyTradeGateway

`RoyaltyTradeGateway` is the canonical trade gateway wrapper for protocol `v3.0`.

It resolves to:
- v2: legacy `TradeGateway` ABI/address
- v3: `RoyaltyTradeGateway` ABI/address

## Import

```typescript
import { RoyaltyTradeGateway } from '@elacity-js/contracts';
```

## Initialization

```typescript
import { ChainId } from '@elacity-js/common';
import { setupContracts, RoyaltyTradeGateway } from '@elacity-js/contracts';

setupContracts({ version: '3.0' });

const gateway = RoyaltyTradeGateway.fromChainId(ChainId.Base, adapter);
```

## Core Trading Methods

```typescript
await gateway.sellToken(contractAddress, tokenId, quantity, pricePerToken, payToken);
await gateway.buyToken(seller, contractAddress, tokenId, quantity, value);
await gateway.withdrawListing(contractAddress, tokenId, quantity);
```

## Offer Lifecycle

```typescript
await gateway.createOfferERC20(contractAddress, tokenId, quantity, pricePerToken, payToken);
await gateway.createOfferNative(contractAddress, tokenId, quantity, pricePerToken, value);
await gateway.acceptOffer(offerer, contractAddress, tokenId, quantity);
await gateway.cancelOffer(contractAddress, tokenId);
```

## Queries

```typescript
const [qty, unitPrice, payToken] = await gateway.listings(contractAddress, tokenId, seller);
const sellersForToken = await gateway.sellersOf(contractAddress, tokenId);
const allSellers = await gateway.sellersOf(contractAddress); // v3 only
const storageAddress = await gateway.store(); // resolves store() on v2, cstore() on v3
```

## Notes

- `TradeGateway` remains available as a compatibility alias.
- For new v3 integrations, prefer `RoyaltyTradeGateway`.
