# Channel Factory

`ChannelFactory` is the V3 channel-creation router.

## Responsibilities

- route channel creation by `(channelType, scope)`,
- delegate deployment to concrete factories,
- serve as the canonical channel-creation entrypoint for integrations.

## Common method

- `createChannel(...)`

## Example

```typescript
const txReq = await channelFactory.createChannel(channelType, scope, name, tokenURI, initData, value);
const tx = await txReq.commit();
await tx.wait();
```

## Migration note

Use `ChannelFactory` instead of legacy `ChannelCore`.
