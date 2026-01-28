# MultiChannel

The `MultiChannel` class provides a typed wrapper for interacting with Elacity MultiChannel contracts. MultiChannel is a specialized channel type that supports linking to other existing channels, allowing aggregation of content from multiple sources.

**Key Difference from StandardChannel:** MultiChannel does **not** support minting new tokens. Instead, it wraps existing channels to create a unified subscription experience.

## Import

```typescript
import { MultiChannel } from '@elacity-js/contracts';
```

## Initialization

```typescript
const multiChannel = new MultiChannel(contractAddress, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the MultiChannel contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## Channel Wrapping

### Wrap Channel

Link an existing channel into this MultiChannel.

```typescript
const tx = await multiChannel.wrapChannel(existingChannelAddress);
```

**Returns:** `Promise<TransactionResponse>`

**Note:** This allows the MultiChannel to aggregate content from multiple StandardChannels, providing subscribers access to all wrapped channels through a single subscription.

## Subscription Management

### Subscribe to Plan

Subscribe to a subscription plan.

```typescript
const tx = await multiChannel.subscribePlan(
  planId,      // Plan ID (uint8)
  recurring,   // Whether subscription is recurring (boolean)
  value        // Optional: native value to send (for native payment)
);
```

**Returns:** `Promise<TransactionResponse>`

### Check Active Subscription

Check if an account has an active subscription.

```typescript
const hasActive = await multiChannel.hasActiveSubscription(subscriberAddress);
```

**Returns:** `Promise<boolean>`

### Unsubscribe from Plan

Unsubscribe from a subscription plan.

```typescript
const tx = await multiChannel.unsubscribePlan(planId);
```

**Returns:** `Promise<TransactionResponse>`

### Get All Plans

Retrieve all subscription plans.

```typescript
const plans = await multiChannel.getPlans();
// Returns: Array<{ planId, payToken, price, duration, active }>
```

**Returns:** `Promise<Array<{ planId: number; payToken: string; price: bigint; duration: bigint; active: boolean }>>`

### Get Plan by ID

Retrieve a specific plan by its ID.

```typescript
const plan = await multiChannel.plans(planId);
// Returns: { planId, payToken, price, duration, active }
```

**Returns:** `Promise<{ planId: number; payToken: string; price: bigint; duration: bigint; active: boolean }>`

### Get Next Plan ID

Get the next available plan ID.

```typescript
const nextId = await multiChannel.nextPlanId();
```

**Returns:** `Promise<number>`

## Rewards Management

### Withdraw Rewards

Withdraw accrued rewards for a specific payment token.

```typescript
const tx = await multiChannel.withdrawRewards(paymentTokenAddress);
```

**Returns:** `Promise<TransactionResponse>`

### Get Rewards Balance

Get the rewards balance for a specific user and token.

```typescript
const rewards = await multiChannel.rewardsOf(userAddress, tokenAddress);
```

**Returns:** `Promise<bigint>`

### Get Rewards (Alternative)

Alternative method to get rewards balance (legacy method name).

```typescript
const rewards = await multiChannel.rewards(userAddress, tokenAddress);
```

**Returns:** `Promise<bigint>`

## Differences from StandardChannel

| Feature | StandardChannel | MultiChannel |
| :--- | :--- | :--- |
| **Minting** | ✅ Supported | ❌ Not supported |
| **Channel Wrapping** | ❌ Not supported | ✅ Supported |
| **Subscription Management** | ✅ Supported | ✅ Supported |
| **Rewards Management** | ✅ Supported | ✅ Supported |
| **ERC-1155 Methods** | ✅ Full support | ✅ Full support |

## Use Cases

MultiChannel is ideal for:
- **Content Aggregators**: Combine multiple creator channels into a single subscription
- **Curated Collections**: Create subscription bundles from existing channels
- **Cross-Channel Access**: Provide subscribers access to multiple channels through one subscription

## Example: Creating a MultiChannel Bundle

```typescript
import { MultiChannel } from '@elacity-js/contracts';
import { ViemAdapter } from '@elacity-js/contracts-viem-adapter';

// Initialize MultiChannel
const multiChannel = new MultiChannel(multiChannelAddress, adapter);

// Wrap existing channels
await multiChannel.wrapChannel(channel1Address);
await multiChannel.wrapChannel(channel2Address);
await multiChannel.wrapChannel(channel3Address);

// Subscribers can now access all wrapped channels
await multiChannel.subscribePlan(planId, true);
```
