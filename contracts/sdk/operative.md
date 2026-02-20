# Operatives

Operative contracts are specialized ERC-1155 contracts that manage access rights,
royalty distribution, and distribution rights for a specific digital asset.
Each digital asset published on a StandardChannel gets its own Operative contract.

## Overview

Operatives are deployed automatically when a StandardChannel creates a new asset. They manage three built-in token IDs:

| ID | Name | Description |
| :--- | :--- | :--- |
| **1** | `ACCESS_TOKEN` | Grants playback / consumption rights for the asset. |
| **2** | `ROYALTY_SHARE` | Entitles holders to a proportional share of sale revenue. |
| **3** | `DISTRIBUTION_RIGHT` | Authorises the holder to sell or trade access tokens. |

### Operative types

| Type | Class | Behaviour |
| :--- | :--- | :--- |
| `1` | `OperativeBuyable` | Buy-play: one-time purchase grants permanent access. |
| `2` | `OperativeBuyableSellable` | Buy-play-sell: access holders can also resell their token on the secondary market. |

The base `Operative` class covers the full common surface. Use a typed subclass when you need variant-specific methods (`setupDistributionRights`, `setResellerCut`).

## Import

```typescript
import {
  Operative,
  OperativeBuyable,
  OperativeBuyableSellable,
} from '@elacity-js/contracts';
```

## Initialization

```typescript
// Base class — works for any operative type
const operative = new Operative(operativeAddress, runner);

// Typed subclasses
const buyable = new OperativeBuyable(operativeAddress, runner);
const buyableSellable = new OperativeBuyableSellable(operativeAddress, runner);
```

### Parameters

- `operativeAddress` (`string`): The deployed address of the Operative contract.
- `runner` (`IContractRunner`): An instance of an adapter (e.g. `EthersAdapter`, `ViemAdapter`).

## Resolving the Operative address

The easiest way to find an operative's address is via the `AuthorityGateway`:

```typescript
const operativeAddress = await authorityGateway.operative(channelAddress, tokenId);
```

Alternatively, parse it from the token metadata:

```typescript
const tokenUri = await channel.uri(tokenId);
const metadata = await fetch(tokenUri).then(res => res.json());
const operativeAddress = metadata.properties.operative;
```

## Token-type constants

```typescript
const ACCESS_TOKEN_ID        = await operative.ACCESS_TOKEN();        // 1n
const ROYALTY_SHARE_ID       = await operative.ROYALTY_SHARE();       // 2n
const DISTRIBUTION_RIGHT_ID  = await operative.DISTRIBUTION_RIGHT();  // 3n
const opType                 = await operative.OP_TYPE();             // 1 or 2
```

## Identity

```typescript
const contentId  = await operative.contentId();    // bytes16 hex – links to the parent asset
const metaURI    = await operative.metadataURI();  // contract.json URI
const uri        = await operative.uri(tokenId);   // per-token metadata URI
const storage    = await operative.dataStorage();  // shared ecosystem storage address
```

## Access & trade checks

### Check access levels

Returns which token IDs `account` holds and whether they grant access.

```typescript
const levels = await operative.checkAccess(accountAddress);
// Array<{ haveAccess: boolean, entitlement: bigint }>

const hasAccess = levels[0].haveAccess; // ACCESS_TOKEN
const hasDistributionRight = levels[1].haveAccess; // DISTRIBUTION_RIGHT
```

### Check trade access

Returns `true` if `account` is allowed to trade `tkId` (e.g. ROYALTY_SHARE requires holding an access token or royalty share).

```typescript
const canTrade = await operative.hasTradeAccess(account, ROYALTY_SHARE_ID);
```

## ERC-1155 methods

Since `Operative` extends `ERC1155`, all standard ERC-1155 methods are available.

### Balances

```typescript
const balance = await operative.balanceOf(account, ACCESS_TOKEN_ID);
const balances = await operative.balanceOfBatch([account1, account2], [id1, id2]);
```

### Transfers

```typescript
const tx = await operative.safeTransferFrom(from, to, ACCESS_TOKEN_ID, 1n, '0x');
await tx.commit();

const batchTx = await operative.safeBatchTransferFrom(from, to, [id1, id2], [amt1, amt2], '0x');
await batchTx.commit();
```

### Approvals

```typescript
const isApproved = await operative.isApprovedForAll(owner, operator);
await operative.setApprovalForAll(operator, true).then(tx => tx.commit());
```

### Supply

```typescript
const exists    = await operative.exists(ACCESS_TOKEN_ID);
const supply    = await operative.totalSupply(ACCESS_TOKEN_ID);
const aggregate = await operative.totalSupply(); // sum across all IDs
```

## Royalty information

Returns how a sale price is split among royalty-share holders.

```typescript
const royalties = await operative.royaltyInfo(salePrice);
// Array<{ receiver: string, amount: bigint }>
```

## Minting

Batch-mint tokens to multiple recipients in one call (factory / ecosystem use).

```typescript
await operative.mintBatchEveryone(
  [addr1, addr2],           // recipients
  [ROYALTY_SHARE_ID, ...],  // token IDs
  [500n, 500n],             // amounts
  '0x'
).then(tx => tx.commit());
```

## Transfer authorisation

Acknowledged ecosystem contracts (e.g. `AuthorityGateway`) need explicit permission
to move access tokens on behalf of holders.

```typescript
// Grant permission
await operative.allowTransferOf(authorityGatewayAddress, ACCESS_TOKEN_ID).then(tx => tx.commit());

// Query permission
const allowed = await operative.allowedTransfer(authorityGatewayAddress, ACCESS_TOKEN_ID);
```

## Rewards

```typescript
const accrued = await operative.rewardsOf(userAddress, paymentTokenAddress);
await operative.withdrawRewards(paymentTokenAddress).then(tx => tx.commit());

// Admin: increment rewards for a user
await operative.incrementRewards(userAddress, amount, paymentTokenAddress).then(tx => tx.commit());
```

## Payment processor

```typescript
const processor = await operative.paymentProcessor();
await operative.setPaymentProcessor(newProcessorAddress).then(tx => tx.commit());
```

## Ownership

```typescript
const ownerAddr = await operative.owner();
await operative.transferOwnership(newOwner).then(tx => tx.commit());
await operative.renounceOwnership().then(tx => tx.commit()); // irreversible
```

---

## OperativeBuyable (type 1)

"Buy once, play always." Purchasers receive an `ACCESS_TOKEN` that grants permanent playback rights.
Only the creator holds a `DISTRIBUTION_RIGHT` token, making them the sole party authorised to sell access tokens.

```typescript
const buyable = new OperativeBuyable(operativeAddress, runner);

// Called by the factory right after proxy initialisation
await buyable.setupDistributionRights(creatorAddress).then(tx => tx.commit());
```

---

## OperativeBuyableSellable (type 2)

"Buy, play, and resell." Access-token holders can resell on the secondary market.
Distribution rights automatically migrate to the new holder on transfer.

```typescript
const buyableSellable = new OperativeBuyableSellable(operativeAddress, runner);

// Called by the factory right after proxy initialisation
await buyableSellable.setupDistributionRights(creatorAddress).then(tx => tx.commit());

// Query the current reseller cut (basis points, 1000 = 100 %)
const cut = await buyableSellable.resellerCut(); // e.g. 200 = 20 %

// Update to a new cut (owner or distributor only)
await buyableSellable.setResellerCut(150).then(tx => tx.commit()); // 15 %
```

### Reseller cut reference

| Value | Percentage |
| :--- | :--- |
| `0` | 0 % (reseller keeps nothing) |
| `200` | 20 % |
| `500` | 50 % |
| `1000` | 100 % (platform/royalties get 0 %) |

---

## Example: Transfer a royalty share

```typescript
const ROYALTY_SHARE_ID = await operative.ROYALTY_SHARE(); // total supply = 1000 (100 %)

// Transfer 10 % (100 units)
const tx = await operative.safeTransferFrom(
  ownerAddress,
  recipientAddress,
  ROYALTY_SHARE_ID,
  100n,
  '0x'
);
await tx.commit();
```
