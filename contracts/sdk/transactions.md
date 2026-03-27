# Transaction Handling

State-changing SDK methods return committable transactions.

## Pattern

```typescript
const action = await contract.method(...args);
const tx = await action.commit();
await tx.wait();
```

## Why this pattern

- supports composition/executor flows,
- enables delayed submission,
- keeps call construction separate from execution strategy.

## Executor usage

For batched or smart-account execution, use your configured transaction executor abstraction and pass the committable actions into it.
