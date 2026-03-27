# Operative Wrappers

Operatives manage asset-level rights and royalty-share logic.

## Variants

- `OperativeBuyable` (type 1)
- `OperativeBuyableSellable` (type 2)

## Common methods

- `checkAccess(account)`
- `royaltyInfo(salePrice)`
- `withdrawRewards(paymentToken)`
- standard ERC-1155 read/write methods

## Type-2 specific

- `setResellerCut(...)`
- `resellerCut()`

## Example

```typescript
const hasAccess = await operative.checkAccess(user);
const royalties = await operative.royaltyInfo(price);
```
