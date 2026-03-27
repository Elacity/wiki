# MultiChannel Wrapper

`MultiChannel` behaves like a channel ledger with wrapper/aggregation semantics.

## Common methods

- `subscribePlan(uint8 planId, bytes args)`
- `unsubscribePlan(uint8 planId)`
- `hasActiveSubscription(address)`
- `wrapChannel(address)` (admin/configuration flow)
- `withdrawRewards(address)`

## Example

```typescript
const subscribeTx = await multiChannel.subscribePlan(planId, '0x');
await (await subscribeTx.commit()).wait();
```

## Notes

- MultiChannel subscription/access behavior is coordinated with `ChannelRegistry` relationships.
- Plan and token-access policy updates remain permissioned operations.
