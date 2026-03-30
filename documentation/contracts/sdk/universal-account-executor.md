# Universal Account Executor

The `@elacity-js/contracts-ua-executor` package provides a specialized `ITransactionExecutor` that leverages Particle Network's Universal Account to execute transactions.

This executor allows you to bundle multiple contract calls into a single meta-transaction, which is then executed by a smart account. This is particularly useful for improving user experience and enabling cross-chain operations through Particle's infrastructure.

## Installation

```bash
npm install @elacity-js/contracts-ua-executor @particle-network/universal-account-sdk
```

## Setup and Usage

To use the `UniversalAccountTransactionExecutor`, you need a `UniversalAccount` instance from Particle Network and a signing function.

```typescript
import { UniversalAccountTransactionExecutor } from '@elacity-js/contracts-ua-executor';
import { EthersAdapter } from '@elacity-js/contracts-ethers-adapter';

// 1. Initialize the executor
const executor = new UniversalAccountTransactionExecutor(runner, {
  ua: particleUaInstance,
  chainId: 8453, // Target chain ID (e.g., Base)
  signMessage: async (message) => {
    // Implement your signing logic here (e.g., using an EOA)
    return await signer.signMessage(message);
  },
});

// 2. Execute bundled transactions
const response = await executor.execute([
  channel.mint(uri, opType, opRawData, sellRawData),
  operative.setApprovalForAll(operator, true),
]);

console.log('Bundle transaction ID:', response.transactionId);

// 3. Wait for completion
await Promise.all(
  response.transactions.map(
    async (tx) => tx.wait()
  )
)
```

## Key Features

- **Transaction Bundling**: Automatically packages multiple SDK contract calls into a single Particle "Universal Transaction".
- **Smart Account Integration**: Executes transactions through the user's Particle Smart Account.
- **Dry-Run Mode**: Seamlessly handles the transition from raw ABI encoding (dry-run) to actual execution.
- **Token Expectations**: (Optional) Specify tokens that the smart account should have available before executing the transaction.

## Configuration Options

The `UAExecutorConfig` object accepts the following properties:

- `ua`: An instance of `UniversalAccount`.
- `chainId`: The target chain ID where the transaction will be executed.
- `signMessage`: A function that takes a `rootHash` (string) and returns a signature (string).
- `expectTokens`: (Optional) An array of tokens and amounts that the account is expected to hold.

## Response Structure

The `execute` method returns a `UniversalxTransactionResult` which includes:
- `transactionId`: The Particle transaction identifier.
- `transactions`: An array following the `IContractTransactionResponse` interface for compatibility with the SDK's waiting mechanism.
- Other Particle-specific transaction details (status, type, etc.).
