# Transaction Handling

The Elacity Contracts SDK provides a flexible way to handle state-changing transactions. With the introduction of the `ICommitableContractTransaction` interface, developers have more control over when and how transactions are submitted to the blockchain.

## The ICommitableContractTransaction Interface

When you call a state-changing method on a contract wrapper (e.g., `channel.mint(...)`), it no longer submits the transaction immediately. Instead, it returns an object implementing `ICommitableContractTransaction`.

This object contains:
- `raw`: The raw ABI-encoded call data (`IContractCallArgs`).
- `commit()`: A function that submits the transaction on-chain via the runner and returns a promise resolving to an `IContractTransactionResponse`.

## Two Ways to Execute Transactions

### 1. Direct Commitment

The simplest way to execute a transaction is to call `.commit()` directly on the returned object. This is ideal for single, standalone operations.

```typescript
// 1. Prepare the transaction (StandardChannel.mint is a state-changing operation)
const commitTx = await channel.mint(uri, opType, opRawData, sellRawData);

// 2. Submit it on-chain
const tx = await commitTx.commit();

// 3. Wait for it to be mined
const receipt = await tx.wait();
console.log('Transaction successful:', receipt.transactionHash);
```

### 2. Executor-based Execution

For more advanced use cases, such as bundling multiple operations or using a smart account for execution, you can use an `ITransactionExecutor`.

The executor sits above the `IContractRunner` and can take multiple committable transactions, handle them according to its specific logic (e.g., bundling them into a single meta-transaction), and then execute them.

```typescript
import { StandardTransactionExecutor } from '@elacity-js/contracts';

const executor = new StandardTransactionExecutor(runner);

// Execute multiple transactions through the executor
const response = await executor.execute([
  channel.mint(uri, opType, opRawData, sellRawData),
  operative.setApprovalForAll(operator, true),
]);

// Wait for all transactions to complete
await Promise.all(response.transactions.map(tx => tx.wait()));
```

## Why use an Executor?

- **Bundling**: Group multiple contract calls into a single atomic transaction (depending on the executor implementation).
- **Abstraction**: Write your business logic once and switch between different execution strategies (standard vs. smart account) just by changing the executor.
- **Gas Optimization**: Some executors can optimize gas usage for multiple operations.
- **Enhanced UX**: Provide a better experience for users by reducing the number of signing prompts.

For a specialized implementation of the executor pattern, see the [**Universal Account Executor**](universal-account-executor.md) documentation.
