# StandardChannel

The `StandardChannel` class is the primary wrapper for interacting with Elacity Standard Channels (both Public and Private). It combines ERC-1155 multi-token functionality with subscription management, royalty distribution, access-control, and trade-access gating.

**Note:** This is distinct from `MultiChannel`, which wraps existing channels for bundled subscriptions but does not mint new assets.

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
| `runner` | `IContractRunner` | An adapter instance (e.g. `EthersAdapter`, `ViemAdapter`). |

---

## ERC-1155 methods

### `balanceOf(account, id)`

Returns the balance of token type `id` for `account`.

```typescript
const balance = await channel.balanceOf(accountAddress, tokenId);
// Promise<bigint>
```

### `balanceOfBatch(accounts, ids)`

Batch balance query. `accounts` and `ids` must have the same length.

```typescript
const balances = await channel.balanceOfBatch([addr1, addr2], [id1, id2]);
// Promise<bigint[]>
```

### `safeTransferFrom(from, to, id, amount, data?)`

Transfers `amount` of token `id` from `from` to `to`.

```typescript
await channel.safeTransferFrom(from, to, tokenId, 1n, '0x').then(tx => tx.commit());
```

### `safeBatchTransferFrom(from, to, ids, amounts, data?)`

Batch transfer of multiple token types.

```typescript
await channel.safeBatchTransferFrom(from, to, [id1, id2], [amt1, amt2], '0x').then(tx => tx.commit());
```

### `uri(id)` / `tokenURI(tokenId)`

Returns the metadata URI for a specific token ID.

```typescript
const uri = await channel.uri(tokenId);
const same = await channel.tokenURI(tokenId);
```

### `name()` / `symbol()`

```typescript
const name   = await channel.name();
const symbol = await channel.symbol();
```

### `exists(id)` / `totalSupply(id?)`

```typescript
const exists    = await channel.exists(tokenId);
const supply    = await channel.totalSupply(tokenId);
const aggregate = await channel.totalSupply(); // aggregate across all IDs
```

### `isApprovedForAll(account, operator)` / `setApprovalForAll(operator, approved)`

```typescript
const approved = await channel.isApprovedForAll(owner, operator);
await channel.setApprovalForAll(operator, true).then(tx => tx.commit());
```

---

## Minting

### `mint(uri, opType, opRawData, sellRawData, value?)`

Mints a new digital asset and optionally initialises its protection and trading flow.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `uri` | `string` | Token metadata URI (IPFS-based JSON). |
| `opType` | `number` | Operative type id: `1` = buy-play, `2` = buy-play-sell, `0` = free. |
| `opRawData` | `string` | ABI-encoded operative initialisation data (`'0x'` to skip). |
| `sellRawData` | `string` | ABI-encoded listing data (`'0x'` to skip auto-listing). |
| `value` | `bigint \| string` | Optional: native value for mint fees. |

```typescript
await channel.mint(metadataURI, 1, opRawData, sellRawData).then(tx => tx.commit());
```

---

## Protocol addresses

```typescript
const authorityGateway = await channel.authority();    // AuthorityGateway address
const tradeGatewayAddr = await channel.tradeGateway(); // TradeGateway address
const storageAddr      = await channel.store();         // CoreStorage address
```

---

## Payment processor

```typescript
const processor = await channel.paymentProcessor();
await channel.setPaymentProcessor(newProcessorAddress).then(tx => tx.commit());
```

---

## Trade access

Channels implement `ITradeAccessRestriction`. The `TradeGateway` calls this before allowing a listing or purchase.

```typescript
const canTrade = await channel.hasTradeAccess(account, tokenId);
// true if the account may trade this token on the marketplace
```

---

## Transfer authorisation

```typescript
// Allow a gateway to transfer tokens on behalf of holders
await channel.allowTransferOf(authorityGatewayAddress, tokenId).then(tx => tx.commit());

// Check if allowed
const allowed = await channel.allowedTransfer(authorityGatewayAddress, tokenId);
```

---

## Royalty information

```typescript
const royalties = await channel.royaltyInfo(salePrice);
// Array<{ receiver: string, amount: bigint }>
```

---

## Subscription management

### Subscribe / unsubscribe

```typescript
// ERC-20 payment plan
await channel.subscribePlan(planId, false).then(tx => tx.commit());

// Native currency payment plan
await channel.subscribePlan(planId, false, totalValue).then(tx => tx.commit());

await channel.unsubscribePlan(planId).then(tx => tx.commit());
```

### Check subscription

```typescript
const active = await channel.hasActiveSubscription(subscriberAddress);
```

### Read plans

```typescript
const plans  = await channel.getPlans();          // all plans
const plan   = await channel.plans(planId);        // single plan
const nextId = await channel.nextPlanId();         // next auto-assigned plan ID

// Plan shape
// { planId: number, payToken: string, price: bigint, duration: bigint, active: boolean }
```

### Bulk plan management

Apply a batch of create/update/remove operations in one transaction.
Requires the `PLAN_MANAGER` role.

```typescript
await channel.bulkUpdatePlans([
  { actionType: 'create', args: '0x...' },
  { actionType: 'update', args: '0x...' },
]).then(tx => tx.commit());
```

---

## Rewards

```typescript
const accrued = await channel.rewardsOf(user, paymentToken);
await channel.withdrawRewards(paymentToken).then(tx => tx.commit());

// Admin: increment rewards for a user (ecosystem contracts only)
await channel.incrementRewards(user, amount, paymentToken).then(tx => tx.commit());
```

---

## Token-ownership-based access

Channels can grant access to holders of a specific ERC-20 token without requiring a subscription.

```typescript
// Check if account qualifies via token balance
const qualifies = await channel.checkTokenOwnershipAccess(accountAddress);

// Query the threshold for a specific token
const threshold = await channel.ownershipThreshold(erc20TokenAddress);

// Configure thresholds (requires DEFAULT_ADMIN_ROLE)
await channel.configureTokenOwnershipAccess([
  { tokenAddress: erc20Address, threshold: 100n * 10n ** 18n },
]).then(tx => tx.commit());
```

---

## Access control (roles)

Standard channels use OpenZeppelin AccessControl. Role identifiers:

```typescript
const adminRole    = await channel.DEFAULT_ADMIN_ROLE(); // '0x0000...0000'
const minterRole   = await channel.MINTER_ROLE();        // private channels only
const planManager  = await channel.PLAN_MANAGER();
const royaltyToken = await channel.ROYALTY_TOKEN();
```

Role management:

```typescript
// Check
const isAdmin = await channel.hasRole(adminRole, account);
const admin   = await channel.getRoleAdmin(planManager);

// Grant / revoke / renounce
await channel.grantRole(planManager, account).then(tx => tx.commit());
await channel.revokeRole(planManager, account).then(tx => tx.commit());
await channel.renounceRole(planManager, callerAddress).then(tx => tx.commit());
```
