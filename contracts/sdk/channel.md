# StandardChannel

The `StandardChannel` class is the primary wrapper for interacting with Elacity Standard Channels (both Public and Private). It complies with the ERC-1155 standard and combines subscription management functionality.

**Note:** This is distinct from `MultiChannel`, which supports linking to other existing channels but does not support minting.

## Import

```typescript
import { StandardChannel } from '@elacity-js/contracts';
```

## Initialization

```typescript
const channel = new StandardChannel(contractAddress, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the StandardChannel contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## ERC-1155 Methods

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

### Batch Transfer

Transfer multiple token types in a single transaction.

```typescript
const tx = await channel.safeBatchTransferFrom(
  fromAddress,
  toAddress,
  [tokenId1, tokenId2],  // Array of token IDs
  [amount1, amount2],   // Array of amounts
  '0x'                  // Optional data
);
```

**Returns:** `Promise<TransactionResponse>`

### Get Metadata URI

Retrieve the metadata URI for a specific token.

```typescript
const uri = await channel.uri(tokenId);
```

**Returns:** `Promise<string>`

### Get Token URI

Retrieve the token URI for a specific token ID (alternative to `uri`).

```typescript
const tokenUri = await channel.tokenURI(tokenId);
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

### Token Existence

Check if a token ID exists.

```typescript
const exists = await channel.exists(tokenId);
```

**Returns:** `Promise<boolean>`

### Total Supply

Get the total supply of tokens (all tokens or a specific token ID).

```typescript
// Total supply of all tokens
const total = await channel.totalSupply();

// Total supply of a specific token ID
const tokenTotal = await channel.totalSupply(tokenId);
```

**Returns:** `Promise<bigint>`

## Minting

### Mint New Token

Mint a new token in the channel. **Note:** MultiChannel does not support minting.

```typescript
const tx = await channel.mint(
  uri,           // Token metadata URI
  opType,        // Operative type (uint16)
  opRawData,     // Raw data for the operative (hex string)
  sellRawData    // Raw data for selling (hex string)
);
```

**Returns:** `Promise<TransactionResponse>`

## Subscription Management

### Subscribe to Plan

Subscribe to a subscription plan.

```typescript
const tx = await channel.subscribePlan(
  planId,      // Plan ID (uint8)
  recurring,   // Whether subscription is recurring (boolean)
  value        // Optional: native value to send (for native payment)
);
```

**Returns:** `Promise<TransactionResponse>`

### Check Active Subscription

Check if an account has an active subscription.

```typescript
const hasActive = await channel.hasActiveSubscription(subscriberAddress);
```

**Returns:** `Promise<boolean>`

### Unsubscribe from Plan

Unsubscribe from a subscription plan.

```typescript
const tx = await channel.unsubscribePlan(planId);
```

**Returns:** `Promise<TransactionResponse>`

### Get All Plans

Retrieve all subscription plans.

```typescript
const plans = await channel.getPlans();
// Returns: Array<{ planId, payToken, price, duration, active }>
```

**Returns:** `Promise<Array<{ planId: number; payToken: string; price: bigint; duration: bigint; active: boolean }>>`

### Get Plan by ID

Retrieve a specific plan by its ID.

```typescript
const plan = await channel.plans(planId);
// Returns: { planId, payToken, price, duration, active }
```

**Returns:** `Promise<{ planId: number; payToken: string; price: bigint; duration: bigint; active: boolean }>`

### Get Next Plan ID

Get the next available plan ID.

```typescript
const nextId = await channel.nextPlanId();
```

**Returns:** `Promise<number>`

## Rewards Management

### Withdraw Rewards

Withdraw accrued rewards for a specific payment token.

```typescript
const tx = await channel.withdrawRewards(paymentTokenAddress);
```

**Returns:** `Promise<TransactionResponse>`

### Get Rewards Balance

Get the rewards balance for a specific user and token.

```typescript
const rewards = await channel.rewardsOf(userAddress, tokenAddress);
```

**Returns:** `Promise<bigint>`

### Increment Rewards

Increment rewards for a user (admin function).

```typescript
const tx = await channel.incrementRewards(
  toAddress,      // Recipient address
  amount,         // Amount to increment
  paymentToken    // Payment token address
);
```

**Returns:** `Promise<TransactionResponse>`

## Royalty Information

### Get Royalty Info

Get royalty information for a sale price.

```typescript
const royalties = await channel.royaltyInfo(salePrice);
// Returns: Array<{ receiver, amount }>
```

**Returns:** `Promise<Array<{ receiver: string; amount: bigint }>>`
