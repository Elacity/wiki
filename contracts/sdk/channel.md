# StandardChannel

The `StandardChannel` class is the primary wrapper for interacting with Elacity Standard Channels (both Public and Private). It complies with the ERC-1155 standard and combines subscription management functionality.

**Note:** This is distinct from `MultiChannel`, which supports linking to other existing channels but does not support minting.

## Import

```typescript
import { StandardChannel } from '@elacity-js/contracts';
```

## Initialization

```typescript
const channel = new StandardChannel(contractAddress, runner);
```

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `contractAddress` | `string` | The deployed address of the StandardChannel contract. |
| `runner` | `IContractRunner` | An instance of an adapter (e.g. `EthersAdapter`, `ViemAdapter`). See [Interfaces](../interfaces.md). |

## ERC-1155 Methods

### `balanceOf(account, id): Promise<bigint>`

Retrieve the balance of a specific token ID for an account.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `account` | `string` | Address to check balance for. |
| `id` | `bigint` \| `number` | Token ID to check. |

**Returns:** `Promise<bigint>`

```typescript
const balance = await channel.balanceOf(accountAddress, tokenId);
```

### `balanceOfBatch(accounts, ids): Promise<bigint[]>`

Check balances for multiple accounts and token IDs in a single call.

**Parameters:**
- `accounts`: `string[]` - Array of accounts.
- `ids`: `(bigint | number)[]` - Array of token IDs.

**Returns:** `Promise<bigint[]>`

### `safeTransferFrom(from, to, id, amount, data?): Promise<IContractTransactionResponse>`

Transfer tokens from one account to another.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `from` | `string` | Current owner address. |
| `to` | `string` | Recipient address. |
| `id` | `bigint` \| `number` | Token ID to transfer. |
| `amount` | `bigint` \| `number` | Amount to transfer. |
| `data` | `string` | (Optional) Hex data (default: '0x'). |

**Returns:** `Promise<IContractTransactionResponse>` - See [Interfaces](../interfaces.md#icontracttransactionresponse).

### `safeBatchTransferFrom(from, to, ids, amounts, data?): Promise<IContractTransactionResponse>`

Transfer multiple token types in a single transaction.

**Returns:** `Promise<IContractTransactionResponse>`

### `uri(id): Promise<string>`

Retrieve the metadata URI for a specific token ID.

**Returns:** `Promise<string>`

---

## Minting

### `mint(uri, opType, opRawData, sellRawData): Promise<IContractTransactionResponse>`

Mint a new token in the channel. **Note:** MultiChannel does not support minting.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `uri` | `string` | Token metadata URI (typically ipfs://...). |
| `opType` | `number` | Operative type (uint16). |
| `opRawData` | `string` | Raw data for the operative (hex string). |
| `sellRawData` | `string` | Raw data for selling (hex string). |

**Returns:** `Promise<IContractTransactionResponse>`

---

## Subscription Management

### `subscribePlan(planId, recurring, value?): Promise<IContractTransactionResponse>`

Subscribe to a subscription plan.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `planId` | `number` | Plan ID (uint8). |
| `recurring` | `boolean` | Whether subscription should be recurring. |
| `value` | `bigint` \| `string` | (Optional) Native value to send (for native payment). |

**Returns:** `Promise<IContractTransactionResponse>`

### `hasActiveSubscription(subscriber): Promise<boolean>`

Check if an account has an active subscription.

**Returns:** `Promise<boolean>`

### `getPlans(): Promise<SubscriptionPlan[]>`

Retrieve all subscription plans.

**Returns:** `Promise<SubscriptionPlan[]>`

```typescript
interface SubscriptionPlan {
  planId: number;
  payToken: string;
  price: bigint;
  duration: bigint;
  active: boolean;
}
```

---

## Rewards Management

### `withdrawRewards(paymentToken): Promise<IContractTransactionResponse>`

Withdraw accrued rewards for a specific payment token.

**Returns:** `Promise<IContractTransactionResponse>`

### `rewardsOf(user, token): Promise<bigint>`

Get the rewards balance for a specific user and token.

**Returns:** `Promise<bigint>`
