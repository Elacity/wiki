# AuthorityGateway

The `AuthorityGateway` class provides a typed wrapper for interacting with the Elacity Authority Gateway smart contract. It handles access control checks, license validation, and access token purchases.

## Import

```typescript
import { AuthorityGateway } from '@elacity-js/contracts';
```

## Initialization

```typescript
const gateway = new AuthorityGateway(contractAddress, adapter);
```

### Parameters

- `contractAddress` (`string`): The deployed address of the AuthorityGateway contract.
- `adapter` (`IContractRunner`): An instance of an adapter (e.g., `EthersAdapter`, `ViemAdapter`).

## Methods

### Check Access

Check if an account has a valid license or access token for a specific asset.

```typescript
const hasAccess = await gateway.hasAccess(
  userAddress,      // Address to check
  channelAddress,   // Address of the channel (ledger)
  tokenId           // Token ID to check
);
```

**Returns:** `Promise<boolean>` - `true` if the user has valid access.

### Check Access by Content ID

Verify access using a simplified Content ID (KID) lookup.

```typescript
const hasAccess = await gateway.hasAccessByContentId(
  userAddress,      // Address to check
  contentId         // 128-bit Content ID (hex string)
);
```

**Returns:** `Promise<boolean>`

### Buy Access (ERC-20)

Purchase access tokens using an ERC-20 payment token.

```typescript
const tx = await gateway.buyAccessERC20(
  sellerAddress,    // Address of the seller
  channelAddress,   // Address of the channel
  tokenId,          // Token ID to buy
  quantity,         // Number of tokens
  pricePerToken,    // Price per token (in wei)
  paymentTokenAddr  // Address of the ERC-20 payment token
);
```

**Returns:** `Promise<TransactionResponse>`

### Buy Access (Native)

Purchase access tokens using native currency (ETH/ELA).

```typescript
const tx = await gateway.buyAccessNative(
  sellerAddress,    // Address of the seller
  channelAddress,   // Address of the channel
  tokenId,          // Token ID to buy
  quantity,         // Number of tokens
  pricePerToken,    // Price per token (in wei)
  value             // Total native value to send (must match quantity * price)
);
```

**Returns:** `Promise<TransactionResponse>`
