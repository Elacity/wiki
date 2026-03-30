# Setup & Configuration

Configure the Elacity Media Player before creating player instances. The `setup()` function initializes the WebAssembly runtime, loads the crypto protocol module, and configures DRM system defaults. It must be called **once** before any call to `create()`.

## `setup()`

One-time initialisation function that bootstraps the WebAssembly runtime, loads the crypto protocol module, and registers default DRM system priorities. It **must** be called before any call to `create()`. Subsequent player instances share the runtime initialised here, so you only need to call it once at application startup.

### Signature

```typescript
function setup<T>(options: PlayerInitOptions<T>): Promise<void>
```

### `PlayerInitOptions<T>`

```typescript
interface PlayerInitOptions<T> extends Record<string, any> {
  provider?: T;
  drmSystem?: Partial<Record<DrmSystemType, DrmSystemParameters>>;
  cryptoVersion?: string;
  remote?: boolean;
}
```

### Options

#### `provider` (required in practice)

An EIP-1193 compatible Web3 provider. The player does not construct providers itself; it only uses the injected provider to resolve blockchain operations when needed (e.g. license acquisition, EIP-712 signing).

Supported providers:
- Ethers.js `BrowserProvider`
- Viem `WalletClient`
- Any object implementing the EIP-1193 `request()` interface

```javascript
import { BrowserProvider } from 'ethers';
import { setup } from '@elacity-js/media-player';

await setup({
  provider: new BrowserProvider(window.ethereum),
});
```

The provider must support at least:
- `eth_requestAccounts` – wallet connection
- `eth_signTypedData_v4` – EIP-712 message signing (used for DRM license acquisition)
- `eth_call` – read calls to smart contracts

#### `drmSystem` (optional)

Configures which DRM systems are available and in what order they are attempted. Lower `priority` value means the system is tried first. A system can be disabled entirely with `disabled: true`.

```typescript
type DrmSystemType =
  | 'cenc:web3-drm-v1'
  | 'cenc:lit-drm-v1'
  | 'cenc:lit-drm-sa-v1';

type DrmSystemParameters = {
  /** Lower value = higher priority. Default is 0. */
  priority?: number;
  /** When true the system is skipped entirely. Default is false. */
  disabled?: boolean;
};
```

**Default configuration:**

```javascript
{
  'cenc:lit-drm-sa-v1': { priority: 0 },  // Tried first
  'cenc:lit-drm-v1':    { priority: 1 },   // Tried second
  'cenc:web3-drm-v1':   { priority: 5 },   // Tried last
}
```

**Override example:**

```javascript
await setup({
  provider: window.ethereum,
  drmSystem: {
    'cenc:web3-drm-v1': { priority: 0 },
    'cenc:lit-drm-v1':  { disabled: true },
  },
});
```

This configuration makes the Web3-based DRM the primary system and disables Lit Protocol entirely.

#### `cryptoVersion` (optional)

Version of the crypto protocol WASM module to load. Only relevant when `cenc:web3-drm-v1` is enabled.

**Default:** `"1.0.11"`

```javascript
await setup({
  provider: window.ethereum,
  cryptoVersion: '1.0.12',
});
```

#### `remote` (optional)

When `true`, WASM modules are fetched from a CDN (jsDelivr) instead of being resolved locally. Useful for applications that do not bundle the WASM files.

**Default:** `false`

```javascript
await setup({
  provider: window.ethereum,
  remote: true,
});
```

---

## `setProvider()`

The player itself does not manage wallet connections — it only consumes an externally supplied Web3 provider for blockchain operations such as license acquisition and EIP-712 signing. `setProvider()` lets you swap or update that provider at any point after `setup()` has been called, which is particularly useful when the user switches wallets or accounts.

### Signature

```typescript
function setProvider<T>(provider: T, accountOverride?: string | null): void
```

### Parameters

| Parameter | Type | Description |
|---|---|---|
| `provider` | `T` | New EIP-1193 provider instance |
| `accountOverride` | `string \| null` | Optional address to use instead of requesting accounts from the provider (e.g. a smart-account address) |

### Example

```javascript
import { setProvider } from '@elacity-js/media-player';

// After wallet switch
setProvider(newProvider);

// With a smart-account address
setProvider(newProvider, '0x<smart-account-address>');
```

### Reacting to wallet changes

```javascript
window.ethereum.on('accountsChanged', (accounts) => {
  if (accounts.length > 0) {
    setProvider(window.ethereum);
  }
});
```

---

## Complete Setup Example

```javascript
import { setup, create, setProvider } from '@elacity-js/media-player';
import { BrowserProvider } from 'ethers';

async function initializePlayer(videoElement, tokenAddress, tokenId, manifestUrl) {
  // 1. One-time setup
  await setup({
    provider: new BrowserProvider(window.ethereum),
    drmSystem: {
      'cenc:web3-drm-v1': { priority: 0 },
      'cenc:lit-drm-v1':  { priority: 1 },
    },
  });

  // 2. Create as many player instances as needed
  const player = await create(tokenAddress, tokenId, videoElement, manifestUrl);

  // 3. Update provider later if the wallet changes
  window.ethereum.on('accountsChanged', () => {
    setProvider(window.ethereum);
  });

  return player;
}
```

---

## Error Handling

### Setup errors

`setup()` may throw if the runtime fails to initialise:

```javascript
try {
  await setup({ provider: window.ethereum });
} catch (error) {
  if (error.message.includes('SharedArrayBuffer')) {
    // COOP/COEP headers are missing – required for multi-threaded WASM
    console.error('Cross-Origin-Opener-Policy / Cross-Origin-Embedder-Policy headers not set');
  } else {
    console.error('Player setup failed:', error);
  }
}
```

### Provider errors

Provider-related errors surface as player events after setup. See the [Events](events.md) page for `sign_request`, `sign_error`, and `error` handling.

---

## Best Practices

1. **Call `setup()` once** at application startup. Creating multiple player instances afterwards is fine, but calling `setup()` again is redundant.
2. **Update the provider via `setProvider()`**, not by calling `setup()` a second time.
3. **Configure DRM priorities** based on your deployment: if all your content uses Web3-based DRM, set its priority to `0` and disable others to avoid unnecessary fallback attempts.

## Related

- [Player API](player.md) – creating and controlling player instances
- [Events](events.md) – event handling and error recovery
- [DRM Systems](../architecture/drm-systems.md) – how each DRM system works
