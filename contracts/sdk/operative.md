# Operatives

Operative contracts are specialized ERC-1155 tokens that manage access rights and royalty distribution for specific digital assets. Each digital asset in a StandardChannel has its own Operative contract.

## Overview

Operatives are deployed automatically when a StandardChannel creates a new asset. They are responsible for:
- Issuing `ACCESS_TOKEN`s (Token ID: 1)
- Managing `ROYALTY_SHARE`s (Token ID: 2)
- Handling `DISTRIBUTION_RIGHT`s (Token ID: 3)

The `Operative` class extends `ERC1155` and adds Operative-specific methods.

## Import

```typescript
import { Operative } from '@elacity-js/contracts';
```

## Initialization

```typescript
const operative = new Operative(operativeAddress, adapter);
```

### Parameters

- `operativeAddress` (`string`): The deployed address of the Operative contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## Resolving Operative Address

To interact with an Operative, you first need its contract address. This can be found in the token metadata of the StandardChannel asset.

```typescript
// Assuming you have a StandardChannel instance
const tokenUri = await channel.uri(tokenId);
const metadata = await fetch(tokenUri).then(res => res.json());

const operativeAddress = metadata.properties.operative;
console.log('Operative Address:', operativeAddress);
```

Alternatively, you can use the AuthorityGateway to get the operative address:

```typescript
const operativeAddress = await authorityGateway.operative(channelAddress, tokenId);
```
<｜tool▁calls▁begin｜><｜tool▁call▁begin｜>
glob_file_search

## Token Type Constants

### Get Token Type IDs

Get the token IDs for the three main token types.

```typescript
const ACCESS_TOKEN_ID = await operative.ACCESS_TOKEN();        // Typically 1
const ROYALTY_SHARE_ID = await operative.ROYALTY_SHARE();      // Typically 2
const DISTRIBUTION_RIGHT_ID = await operative.DISTRIBUTION_RIGHT(); // Typically 3
```

**Returns:** `Promise<bigint>` for each method

### Get Operative Type

Get the operative type identifier.

```typescript
const opType = await operative.OP_TYPE();
```

**Returns:** `Promise<number>`

## Access Management

### Check Access Token Balance

Check if a user owns an Access Token (ID: 1).

```typescript
const ACCESS_TOKEN_ID = await operative.ACCESS_TOKEN();
const balance = await operative.balanceOf(userAddress, ACCESS_TOKEN_ID);

if (balance > 0n) {
  console.log('User has access!');
}
```

**Returns:** `Promise<bigint>`

### Check Access Levels

Check access levels and entitlements for an account.

```typescript
const accessLevels = await operative.checkAccess(accountAddress);
// Returns: Array<{ haveAccess: boolean, entitlement: bigint }>
```

**Returns:** `Promise<Array<{ haveAccess: boolean; entitlement: bigint }>>`

### Get Content ID

Get the content ID associated with this operative.

```typescript
const contentId = await operative.contentId();
```

**Returns:** `Promise<string>` - 128-bit Content ID (hex string)

## ERC-1155 Methods

Since `Operative` extends `ERC1155`, all standard ERC-1155 methods are available:

### Balance Queries

```typescript
// Single balance
const balance = await operative.balanceOf(account, tokenId);

// Batch balances
const balances = await operative.balanceOfBatch(
  [account1, account2],
  [tokenId1, tokenId2]
);
```

### Transfers

```typescript
// Single transfer
await operative.safeTransferFrom(
  fromAddress,
  toAddress,
  ACCESS_TOKEN_ID,
  amount,
  '0x'
);

// Batch transfer
await operative.safeBatchTransferFrom(
  fromAddress,
  toAddress,
  [ROYALTY_SHARE_ID, DISTRIBUTION_RIGHT_ID],
  [amount1, amount2],
  '0x'
);
```

### Approvals

```typescript
// Check approval
const isApproved = await operative.isApprovedForAll(owner, operator);

// Set approval
await operative.setApprovalForAll(operator, true);
```

### URI

```typescript
const uri = await operative.uri(tokenId);
```

## Royalty Information

### Get Royalty Info

Get royalty information for a sale price.

```typescript
const royalties = await operative.royaltyInfo(salePrice);
// Returns: Array<{ receiver, amount }>
```

**Returns:** `Promise<Array<{ receiver: string; amount: bigint }>>`

## Token Types Reference

| ID | Name | Description |
| :--- | :--- | :--- |
| **1** | `ACCESS_TOKEN` | Grants access to the digital content. Required for decryption. |
| **2** | `ROYALTY_SHARE` | Represents a share of future revenue (1/1000th of total). |
| **3** | `DISTRIBUTION_RIGHT` | Grants the right to sell access tokens in the marketplace. |

## Example: Transfer Royalty Share

```typescript
const ROYALTY_SHARE_ID = await operative.ROYALTY_SHARE(); // Total supply is 1000 (100%)

// Transfer 10% royalty share (100 units)
await operative.safeTransferFrom(
  ownerAddress,
  recipientAddress,
  ROYALTY_SHARE_ID,
  100,
  '0x'
);
```
