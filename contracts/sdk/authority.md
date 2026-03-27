# Authority Gateway

`AuthorityGateway` is the access-token marketplace and access-resolution entrypoint.

## Core responsibilities

- list access tokens for sale,
- buy access tokens using native or ERC-20 paths,
- query access by `(ledger, tokenId)` or `contentId`.

## Common write methods

- `sellAccess(...)`
- `buyAccess(...)` (native)
- `buyAccess(...)` (ERC-20)
- `withdrawListing(...)`

## Common read methods

- `hasAccess(...)`
- `hasAccessByContentId(...)`
- `listings(...)`
- `sellersOf(...)`
- `operative(...)`

## Example

```typescript
const listingTx = await authority.sellAccess(ledger, tokenId, qty, pricePerToken, payToken);
await (await listingTx.commit()).wait();

const buyTx = await authority.buyAccess(seller, ledger, tokenId, qty, pricePerToken, payToken);
await (await buyTx.commit()).wait();

const has = await authority.hasAccessByContentId(user, contentId);
```

## Notes

- Access resolution may include subscription-backed access depending on channel support.
- Internal on-behalf flows are restricted to authorized ecosystem contracts.
