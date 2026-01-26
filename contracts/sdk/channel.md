# Channels (DigitalAsset)

The `DigitalAsset` class is the primary wrapper for interacting with Elacity Channels (both Public and Private). It complies with the ERC-1155 standard and adds Elacity-specific functionality.

## Import

```typescript
import { DigitalAsset } from '@elacity-js/contracts';
```

## Initialization

```typescript
const channel = new DigitalAsset(contractAddress, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the Channel contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## Methods

### Get Token Balance

Retrieve the balance of a specific token ID for an account.

```typescript
const balance = await channel.balanceOf(
  accountAddress, // Address to check
  tokenId         // Token ID
);
console.log(`Balance: ${balance.toString()}`);
```

**Returns:** `Promise<bigint>`

### Batch Balance Check

Check balances for multiple accounts and token IDs in a single call.

```typescript
const balances = await channel.balanceOfBatch(
  [account1, account2], // Array of accounts
  [id1, id2]            // Array of token IDs
);
```

**Returns:** `Promise<bigint[]>`

### Safe Transfer

Transfer tokens from one account to another.

```typescript
const tx = await channel.safeTransferFrom(
  fromAddress,    // Current owner
  toAddress,      // Recipient
  tokenId,        // Token ID
  amount,         // Amount to transfer
  data            // Optional data (default: '0x')
);
```

**Returns:** `Promise<TransactionResponse>`

### Get Metadata URI

Retrieve the metadata URI for a specific token.

```typescript
const uri = await channel.uri(tokenId);
```

**Returns:** `Promise<string>`

### Metadata

Retrieve contract-level metadata.

```typescript
const name = await channel.name();
const symbol = await channel.symbol();
```

### Approval Management

Manage operator approvals for asset transfers.

```typescript
// Check if an operator is approved for all assets
const isApproved = await channel.isApprovedForAll(owner, operator);

// Set approval for an operator
await channel.setApprovalForAll(operator, true); // true to approve, false to revoke
```
