# Royalty Trade Gateway

`RoyaltyTradeGateway` is the V3 ERC-1155 trading gateway for non-access-token marketplace flows.

## Responsibilities

- `sellToken`, `buyToken`, `withdrawListing`,
- offer lifecycle (`createOffer`, `acceptOffer`, `cancelOffer`),
- listing/seller reads.

## Example

```typescript
const sellTx = await royaltyTradeGateway.sellToken(tokenContract, tokenId, qty, price, payToken);
await (await sellTx.commit()).wait();

const buyTx = await royaltyTradeGateway.buyToken(seller, tokenContract, tokenId, qty, value);
await (await buyTx.commit()).wait();
```

## Migration note

Use `RoyaltyTradeGateway` instead of legacy `TradeGateway` naming.
