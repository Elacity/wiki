# Universal Account Executor

This page documents smart-account executor usage at a high level.

## Overview

A universal-account executor can bundle multiple committable SDK actions into a single higher-level execution flow.

## Typical flow

1. build committable actions,
2. pass them to the executor,
3. wait for resulting transaction responses.

## Example

```typescript
const response = await executor.execute([
  channel.mint(uri, opType, opRawData, sellRawData),
  operative.setApprovalForAll(operator, true),
]);

await Promise.all(response.transactions.map((tx) => tx.wait()));
```

## Note

Ensure chain IDs and account configuration match the deployed V3 environment.
