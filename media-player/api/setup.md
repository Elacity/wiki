# Setup & Configuration

Configure the Elacity Media Player before creating player instances.

## `setup()`

One-time initialization function. Must be called before creating any player instances.

### Signature

```typescript
function setup<T>(options: PlayerInitOptions<T>): Promise<void>
```

### Parameters

```typescript
interface PlayerInitOptions<T> {
  cryptoVersion?: string;
  provider?: T;
  remote?: boolean;
  ENABLE_FS_LOGGING?: boolean;
  "go.glueCode"?: boolean;
  drmSystem?: Partial<DrmSystemType, DrmSystemParameters>;
}
```

### Options

#### `provider` (required)
Web3 provider for blockchain interactions.

**Supported Providers:**
- Ethers.js `BrowserProvider`
- Viem `WalletClient`
- Any EIP-1193 compatible provider

**Example:**

```javascript
import { ethers } from 'ethers';

await setup({
  provider: new ethers.BrowserProvider(window.ethereum)
});
```

#### `drmSystem` (optional)
DRM system configuration with priorities.

**Default:**

```javascript
{
  'cenc:web3-drm-v1': { priority: 5 },
  'cenc:lit-drm-v1': { priority: 1 },
  'cenc:lit-drm-sa-v1': { priority: 0 }
}
```

**Example:**

```javascript
await setup({
  provider: window.ethereum,
  drmSystem: {
    'cenc:lit-drm-v1': { priority: 0 },
    'cenc:web3-drm-v1': { priority: 1 }
  }
});
```

#### `cryptoVersion` (optional)
Version of crypto protocol module to load.

**Default:** `"1.0.11"`

**Example:**

```javascript
await setup({
  provider: window.ethereum,
  cryptoVersion: '1.0.12'
});
```

#### `remote` (optional)
Load WASM modules from CDN instead of local files.

**Default:** `false`

**Example:**

```javascript
await setup({
  provider: window.ethereum,
  remote: true  // Load from jsdelivr CDN
});
```

#### `ENABLE_FS_LOGGING` (optional)
Enable file system logging for debugging.

**Default:** `false`

**Example:**

```javascript
await setup({
  provider: window.ethereum,
  ENABLE_FS_LOGGING: true
});
```

#### `"go.glueCode"` (optional)
Load Go WASM glue code (for crypto protocol).

**Default:** `false`

**Example:**

```javascript
await setup({
  provider: window.ethereum,
  "go.glueCode": true
});
```

## `setProvider()`

Update the Web3 provider after setup.

### Signature

```typescript
function setProvider<T>(
  provider: T,
  accountOverride?: string | null
): void
```

### Parameters

- **`provider`**: New Web3 provider
- **`accountOverride`**: Optional smart account address override

### Example

```javascript
import { setProvider } from '@elacity-js/media-player';

// Update provider
setProvider(newProvider);

// Update provider with smart account override
setProvider(newProvider, '0x<smart-account-address>');
```

## Complete Setup Example

```javascript
import { setup, create, setProvider } from '@elacity-js/media-player';
import { ethers } from 'ethers';

async function initializePlayer() {
  // 1. Setup player (one-time)
  await setup({
    provider: new ethers.BrowserProvider(window.ethereum),
    drmSystem: {
      'cenc:lit-drm-v1': { priority: 0 },
      'cenc:web3-drm-v1': { priority: 1 }
    },
    remote: false,
    ENABLE_FS_LOGGING: false
  });
  
  // 2. (Optional) Update provider later
  const newProvider = new ethers.BrowserProvider(anotherWallet);
  setProvider(newProvider);
  
  // 3. Create player instances
  const player = await create(
    tokenAddress,
    tokenId,
    videoElement,
    manifestUrl
  );
}
```

## DRM System Configuration

### Priority System

Lower priority number = higher priority. Systems are tried in order:

```javascript
{
  'cenc:lit-drm-sa-v1': { priority: 0 },  // Tried first
  'cenc:lit-drm-v1': { priority: 1 },     // Tried second
  'cenc:web3-drm-v1': { priority: 5 }     // Tried last
}
```

### Per-Playback Override

You can override DRM system per playback:

```javascript
await player.play({
  drmSystem: {
    'cenc:web3-drm-v1': { priority: 0 }  // Override for this playback
  }
});
```

## Provider Requirements

### EIP-1193 Interface

The provider must implement:

```typescript
interface EIP1193Provider {
  request(args: { method: string; params?: any[] }): Promise<any>;
  on(event: string, handler: Function): void;
  removeListener(event: string, handler: Function): void;
}
```

### Required Methods

- `eth_requestAccounts`: Request wallet connection
- `eth_signTypedData_v4`: Sign EIP-712 messages
- `eth_call`: Read from smart contracts

### WalletConnect Support

WalletConnect providers are supported:

```javascript
import { WalletConnectProvider } from '@walletconnect/ethereum-provider';

const provider = await WalletConnectProvider.init({
  projectId: 'your-project-id',
  chains: [1],
  showQrModal: true
});

await setup({ provider });
```

## Error Handling

### Setup Errors

```javascript
try {
  await setup({
    provider: window.ethereum
  });
} catch (error) {
  if (error.message.includes('SharedArrayBuffer')) {
    console.error('COOP/COEP headers missing');
  } else if (error.message.includes('provider')) {
    console.error('Invalid provider');
  }
}
```

### Provider Errors

```javascript
player.addEventListener('sign_error', (e) => {
  if (e.detail.error.code === 4001) {
    console.error('User rejected signature');
  }
});
```

## Best Practices

### 1. Single Setup Call

Call `setup()` once at application startup:

```javascript
// ✅ Good
await setup({ provider: window.ethereum });

// Create multiple players
const player1 = await create(...);
const player2 = await create(...);

// ❌ Bad - Don't call setup multiple times
await setup({ provider: window.ethereum });
await setup({ provider: window.ethereum }); // Redundant
```

### 2. Provider Management

Update provider when wallet changes:

```javascript
window.ethereum.on('accountsChanged', (accounts) => {
  if (accounts.length > 0) {
    setProvider(window.ethereum);
  }
});
```

### 3. DRM Priority

Configure DRM systems based on your use case:

```javascript
// NFT marketplace - prioritize Lit Protocol
{
  'cenc:lit-drm-v1': { priority: 0 },
  'cenc:web3-drm-v1': { priority: 1 }
}

// Custom DRM - prioritize Web3
{
  'cenc:web3-drm-v1': { priority: 0 },
  'cenc:lit-drm-v1': { priority: 1 }
}
```

## Related Documentation

- [Player API](player.md) - Player instance API
- [DRM Systems](../architecture/drm-systems.md) - DRM configuration
- [Troubleshooting](../development/troubleshooting.md) - Common setup issues
