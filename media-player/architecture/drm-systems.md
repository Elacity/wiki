# DRM Systems

The Elacity Media Player supports multiple DRM systems with priority-based fallback. This document explains how each system works and how to configure them.

## Supported DRM Systems

### `cenc:lit-drm-v1`
**Lit Protocol-based DRM** - Recommended for most use cases

- Uses Lit Protocol for decentralized key management
- Supports both EOA (Externally Owned Accounts) and Smart Accounts
- Automatic key retrieval based on access conditions
- Best for: NFT-gated content, subscription-based access

### `cenc:lit-drm-sa-v1`
**Lit Protocol with Smart Account** - For smart account wallets

- Same as `cenc:lit-drm-v1` but optimized for smart accounts
- Requires `accountOverride` parameter
- Best for: Smart account wallet users

### `cenc:web3-drm-v1`
**Web3 DRM** - Custom smart contract-based DRM

- Uses AuthorityGateway smart contract
- EIP-712 signature-based authentication
- Direct blockchain interaction
- Best for: Custom DRM implementations

## DRM System Priority

The player tries DRM systems in order of priority (lower number = higher priority):

```javascript
await setup({
  drmSystem: {
    'cenc:lit-drm-sa-v1': { priority: 0 },  // Highest priority
    'cenc:lit-drm-v1': { priority: 1 },
    'cenc:web3-drm-v1': { priority: 5 }     // Lowest priority
  }
});
```

If a DRM system fails, the player automatically tries the next one in priority order.

## Lit Protocol DRM (`cenc:lit-drm-v1`)

### How It Works

1. **PSSH Extraction**: Player extracts PSSH box from DASH manifest
2. **Access Condition**: PSSH contains Lit Protocol access conditions
3. **Key Retrieval**: Lit Protocol SDK retrieves decryption key
4. **Decryption**: Key is used to decrypt media segments

### Setup

```javascript
import { setup } from '@elacity-js/media-player';
import { LitNodeClient } from '@lit-protocol/lit-node-client';

const litClient = new LitNodeClient({
  litNetwork: 'serrano'
});

await litClient.connect();

await setup({
  provider: window.ethereum,
  drmSystem: {
    'cenc:lit-drm-v1': { priority: 0 }
  }
});
```

### Access Conditions

Access conditions are defined in the PSSH box. Common patterns:

```javascript
// NFT ownership
{
  resourceId: { tokenId: "123", contractAddress: "0x..." },
  accessControlConditions: [
    {
      contractAddress: "0x...",
      standardContractType: "ERC721",
      chain: "ethereum",
      method: "ownerOf",
      parameters: ["123"],
      returnValueTest: {
        comparator: "=",
        value: ":userAddress"
      }
    }
  ]
}
```

### Smart Account Support

For smart accounts, use `cenc:lit-drm-sa-v1`:

```javascript
await setup({
  provider: window.ethereum,
  drmSystem: {
    'cenc:lit-drm-sa-v1': { priority: 0 }
  }
});

// When creating player, provide account override
const player = await create(
  tokenAddress,
  tokenId,
  videoElement,
  manifestUrl
);

// Set smart account address
setProvider(window.ethereum, '0x<smart-account-address>');
```

## Web3 DRM (`cenc:web3-drm-v1`)

### How It Works

1. **PSSH Extraction**: Player extracts PSSH box containing AuthorityGateway address
2. **Signature Request**: Player requests EIP-712 signature from wallet
3. **License Request**: Signature sent to AuthorityGateway contract
4. **Key Retrieval**: Contract returns decryption key
5. **Decryption**: Key used to decrypt media

### Setup

```javascript
await setup({
  provider: window.ethereum,
  drmSystem: {
    'cenc:web3-drm-v1': { priority: 0 }
  }
});
```

### PSSH Box Format

The PSSH box must contain:

```json
{
  "protectionType": "cenc:web3-drm-v1",
  "data": {
    "authority": "0x<AuthorityGateway-address>",
    "chainId": 1
  }
}
```

### Signature Flow

```javascript
// Player automatically requests signature
player.addEventListener('sign_request', () => {
  console.log('Signature requested');
});

// Certificate is automatically cached
player.addEventListener('certificate', (e) => {
  const { signature, signer, entity } = e.detail;
  // Cache certificate for reuse
  localStorage.setItem('certificate', JSON.stringify({
    signature,
    signer
  }));
});
```

### Using Cached Certificate

```javascript
const cachedCert = JSON.parse(localStorage.getItem('certificate'));

await player.play({
  certificate: {
    signer: cachedCert.signer,
    signature: cachedCert.signature
  }
});
```

## DRM System Selection

### Automatic Selection

The player automatically selects the best DRM system:

1. Checks PSSH box for available systems
2. Tries systems in priority order
3. Falls back to next system on failure
4. Throws error if all systems fail

### Manual Override

You can override DRM system per playback:

```javascript
await player.play({
  drmSystem: {
    'cenc:web3-drm-v1': { priority: 0 }  // Override setup defaults
  }
});
```

## Error Handling

### License Acquisition Errors

```javascript
player.addEventListener('error', (e) => {
  if (e.detail.code === 'LICENSE_ERROR') {
    // Handle license acquisition failure
    console.error('Failed to acquire license:', e.detail.message);
  }
});
```

### Signature Errors

```javascript
player.addEventListener('sign_error', (e) => {
  console.error('Signature error:', e.detail.error);
  // User rejected signature or wallet error
});
```

## Best Practices

### 1. Priority Configuration

Set priorities based on your use case:

```javascript
// NFT marketplace - prioritize Lit Protocol
{
  'cenc:lit-drm-v1': { priority: 0 },
  'cenc:web3-drm-v1': { priority: 1 }  // Fallback
}

// Custom DRM - prioritize Web3
{
  'cenc:web3-drm-v1': { priority: 0 },
  'cenc:lit-drm-v1': { priority: 1 }  // Fallback
}
```

### 2. Certificate Caching

Cache certificates to avoid repeated signature requests:

```javascript
let cachedCertificate = null;

player.addEventListener('certificate', (e) => {
  cachedCertificate = e.detail;
});

// Reuse certificate
await player.play({
  certificate: cachedCertificate
});
```

### 3. Error Recovery

Implement fallback logic:

```javascript
try {
  await player.play();
} catch (error) {
  if (error.message.includes('license')) {
    // Try alternative DRM system or show error
  }
}
```

## Related Documentation

- [Player API](../api/player.md) - Player configuration options
- [Architecture Overview](overview.md) - Understanding the architecture
- [Troubleshooting](../development/troubleshooting.md) - Common DRM issues
