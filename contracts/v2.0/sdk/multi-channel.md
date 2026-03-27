# MultiChannel

The `MultiChannel` class provides a typed wrapper for interacting with Elacity MultiChannel contracts. A multi-channel bundles one or more standard channels under a single subscription umbrella — subscribers automatically gain access to every wrapped child channel.

**Key difference from StandardChannel:** MultiChannel does **not** mint new digital assets. It wraps existing standard channels and manages its own subscription plans and royalty distribution.

## Import

```typescript
import { MultiChannel } from '@elacity-js/contracts';
```

## Initialization

```typescript
const multi = new MultiChannel(contractAddress, runner);
```

**Parameters:**

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `contractAddress` | `string` | Deployed address of the MultiChannel contract. |
| `runner` | `IContractRunner` | An adapter instance (e.g. `EthersAdapter`, `ViemAdapter`). |

---

## Identity

```typescript
const channelName = await multi.name();
const storageAddr = await multi.store(); // CoreStorage address
```

---

## Channel wrapping

```typescript
// Wrap an existing standard channel into this bundle
await multi.wrapChannel(standardChannelAddress).then(tx => tx.commit());
```

After wrapping, any active subscriber to this multi-channel also gains access to the wrapped channel's assets.

---

## ERC-1155 methods

MultiChannel is itself an ERC-1155 contract (holding royalty shares and subscription tokens).

```typescript
const balance  = await multi.balanceOf(account, tokenId);
const balances = await multi.balanceOfBatch([addr1, addr2], [id1, id2]);
const exists   = await multi.exists(tokenId);
const supply   = await multi.totalSupply(tokenId);

await multi.safeTransferFrom(from, to, tokenId, 1n, '0x').then(tx => tx.commit());
await multi.safeBatchTransferFrom(from, to, ids, amounts, '0x').then(tx => tx.commit());

const approved = await multi.isApprovedForAll(owner, operator);
await multi.setApprovalForAll(operator, true).then(tx => tx.commit());

const uri = await multi.uri(tokenId);
const tokenUri = await multi.tokenURI(tokenId);
```

---

## Payment processor

```typescript
const processor = await multi.paymentProcessor();
await multi.setPaymentProcessor(newProcessorAddress).then(tx => tx.commit());
```

---

## Trade access

```typescript
const canTrade = await multi.hasTradeAccess(account, tokenId);
```

---

## Transfer authorisation

```typescript
await multi.allowTransferOf(operatorAddress, tokenId).then(tx => tx.commit());
const allowed = await multi.allowedTransfer(operatorAddress, tokenId);
```

---

## Royalty information

```typescript
const royalties = await multi.royaltyInfo(salePrice);
// Array<{ receiver: string, amount: bigint }>
```

---

## Subscription management

### Subscribe / unsubscribe

```typescript
// ERC-20 plan
await multi.subscribePlan(planId, false).then(tx => tx.commit());

// Native currency plan
await multi.subscribePlan(planId, false, totalValue).then(tx => tx.commit());

await multi.unsubscribePlan(planId).then(tx => tx.commit());
```

### Check subscription

```typescript
const active = await multi.hasActiveSubscription(subscriberAddress);
// Returns true if active on this multi-channel OR any wrapped child channel
```

### Read plans

```typescript
const plans  = await multi.getPlans();
const plan   = await multi.plans(planId);
const nextId = await multi.nextPlanId();
// { planId: number, payToken: string, price: bigint, duration: bigint, active: boolean }
```

### Bulk plan management

```typescript
await multi.bulkUpdatePlans([
  { actionType: 'create', args: '0x...' },
]).then(tx => tx.commit());
```

---

## Rewards

```typescript
const accrued = await multi.rewardsOf(user, paymentToken);
await multi.withdrawRewards(paymentToken).then(tx => tx.commit());
await multi.incrementRewards(user, amount, paymentToken).then(tx => tx.commit());
```

---

## Token-ownership-based access

```typescript
const qualifies = await multi.checkTokenOwnershipAccess(accountAddress);
const threshold = await multi.ownershipThreshold(erc20TokenAddress);

await multi.configureTokenOwnershipAccess([
  { tokenAddress: erc20Address, threshold: 100n * 10n ** 18n },
]).then(tx => tx.commit());
```

---

## Access control (roles)

```typescript
const adminRole   = await multi.DEFAULT_ADMIN_ROLE();
const planManager = await multi.PLAN_MANAGER();
const royaltyTk   = await multi.ROYALTY_TOKEN();

const isAdmin = await multi.hasRole(adminRole, account);

await multi.grantRole(planManager, account).then(tx => tx.commit());
await multi.revokeRole(planManager, account).then(tx => tx.commit());
await multi.renounceRole(planManager, callerAddress).then(tx => tx.commit());
```

---

## Comparison: StandardChannel vs MultiChannel

| Feature | StandardChannel | MultiChannel |
| :--- | :--- | :--- |
| Minting new assets | ✅ | ❌ |
| Wrap other channels | ❌ | ✅ |
| Subscription management | ✅ | ✅ |
| Royalty distribution | ✅ | ✅ |
| ERC-1155 | ✅ | ✅ |
| Trade access gating | ✅ | ✅ |
| Token-ownership access | ✅ | ✅ |

---

## Example: Creating a bundle

```typescript
import { MultiChannel } from '@elacity-js/contracts';

const multi = new MultiChannel(multiChannelAddress, runner);

// Wrap three existing channels
for (const addr of [channel1, channel2, channel3]) {
  await multi.wrapChannel(addr).then(tx => tx.commit());
}

// A single subscription now covers all three channels
await multi.subscribePlan(0, false, price).then(tx => tx.commit());

const active = await multi.hasActiveSubscription(userAddress); // true
```
