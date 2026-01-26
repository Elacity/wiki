# Operatives

Operative contracts are specialized ERC-1155 tokens that manage access rights and royalty distribution for specific digital assets. Each digital asset in a Channel has its own Operative contract.

## Overview

Operatives are deployed automatically when a Channels creates a new asset. They are responsible for:
- Issuing `ACCESS_TOKEN`s (Token ID: 1)
- Managing `ROYALTY_SHARE`s (Token ID: 2)
- Handling `DISTRIBUTION_RIGHT`s (Token ID: 3)

Since Operatives follow the ERC-1155 standard, you can interact with them using the generic `ERC1155` class provided by the SDK.

## Import

```typescript
import { ERC1155 } from '@elacity-js/contracts';
```

## Usage

### 1. Resolve Operative Address

To interact with an Operative, you first need its contract address. This can be found in the token metadata of the Channel asset.

```typescript
// Assuming you have a DigitalAsset instance for the Channel
const tokenUri = await channel.uri(tokenId);
const metadata = await fetch(tokenUri).then(res => res.json());

const operativeAddress = metadata.properties.operative;
console.log('Operative Address:', operativeAddress);
```

### 2. Initialize Contract

Use the `ERC1155` wrapper to initialize the contract instance.

```typescript
const operative = new ERC1155(operativeAddress, adapter);
```

### 3. Check Access Token Balance

Check if a user owns an Access Token (ID: 1).

```typescript
const ACCESS_TOKEN_ID = 1;
const balance = await operative.balanceOf(userAddress, ACCESS_TOKEN_ID);

if (balance > 0n) {
  console.log('User has access!');
}
```

### 4. Transfer Rights

Transfers work like any standard ERC-1155 token.

```typescript
const ROYALTY_SHARE_ID = 2; // Total supply is 1000 (100%)

// Transfer 10% royalty share (100 units)
await operative.safeTransferFrom(
  ownerAddress,
  recipientAddress,
  ROYALTY_SHARE_ID,
  100,
  '0x'
);
```

### Token Types

| ID | Name | Description |
| :--- | :--- | :--- |
| **1** | `ACCESS_TOKEN` | Grants access to the digital content. Required for decryption. |
| **2** | `ROYALTY_SHARE` | Represents a share of future revenue (1/1000th of total). |
| **3** | `DISTRIBUTION_RIGHT` | Grants the right to sell access tokens in the marketplace. |
