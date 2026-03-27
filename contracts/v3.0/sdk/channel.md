# Channel Wrappers (DigitalAssetPublic / DigitalAssetPrivate)

Channels are ERC-1155 ledgers for asset minting, subscription, and reward interactions.

## Typical methods

### Mint

- `mint(uri, opType, opRawData, sellRawData)`

### Subscription

- `subscribePlan(uint8 planId, bytes args)`
- `unsubscribePlan(uint8 planId)`
- `hasActiveSubscription(address)`
- `bulkUpdatePlans(...)` (manager/admin flows)

### Rewards and utility

- `withdrawRewards(paymentToken)`
- `setApprovalForAll(...)`

## Subscription signature reminder

Use current shape:

```typescript
await channel.subscribePlan(planId, args).then(tx => tx.commit());
```

Where:

- `args = "0x"` for no extra metadata context, or
- `args = abi.encode(string subscriptionTokenURI)` (SDK helper/encoder dependent).

## Example

```typescript
const mintTx = await channel.mint(tokenUri, 1, opRawData, sellRawData);
await (await mintTx.commit()).wait();

const subTx = await channel.subscribePlan(1, '0x');
await (await subTx.commit()).wait();
```
