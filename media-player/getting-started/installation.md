# Installation

Install the Elacity Media Player package from npm to get started with DRM-protected media playback.

## Package Installation

```bash
npm install @elacity-js/media-player
```

Or with yarn:

```bash
yarn add @elacity-js/media-player
```

## Peer Dependencies

The player requires a Web3 provider for blockchain interactions. Install one of the following:

### Ethers.js (Recommended)

```bash
npm install ethers
```

```javascript
import { ethers } from 'ethers';

const provider = new ethers.BrowserProvider(window.ethereum);
```

### Viem

```bash
npm install viem
```

```javascript
import { createWalletClient, custom } from 'viem';

const provider = createWalletClient({
  transport: custom(window.ethereum)
});
```


## Server Configuration

**⚠️ CRITICAL**: The player requires specific HTTP headers for two reasons:

1. **SharedArrayBuffer Support**: Required for WebAssembly multi-threading (pthreads)
2. **Security Requirements**: SharedArrayBuffer requires COOP/COEP headers due to Spectre/Meltdown mitigations

Without these headers, `SharedArrayBuffer` will be `undefined`, causing multi-threading to fail and potentially preventing player initialization.

### Required Headers

```
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
```

### Example Server Configurations

#### Express.js

```javascript
app.use((req, res, next) => {
  res.setHeader('Cross-Origin-Opener-Policy', 'same-origin');
  res.setHeader('Cross-Origin-Embedder-Policy', 'require-corp');
  next();
});
```

#### Nginx

```nginx
add_header Cross-Origin-Opener-Policy same-origin;
add_header Cross-Origin-Embedder-Policy require-corp;
```

#### Vercel

Create `vercel.json`:

```json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        {
          "key": "Cross-Origin-Opener-Policy",
          "value": "same-origin"
        },
        {
          "key": "Cross-Origin-Embedder-Policy",
          "value": "require-corp"
        }
      ]
    }
  ]
}
```

#### Netlify

Create `_headers` file:

```
/*
  Cross-Origin-Opener-Policy: same-origin
  Cross-Origin-Embedder-Policy: require-corp
```

## TypeScript Support

Type definitions are included in the package. No additional `@types` package needed.

```typescript
import { create, setup, ElacityMediaPlayer } from '@elacity-js/media-player';
```

## Verification

### Check MSE Support

```javascript
if (!window.MediaSource || !MediaSource.isTypeSupported) {
  console.error('Media Source Extensions not supported');
  // Player will not work without MSE
}
```

### Check SharedArrayBuffer Support

```javascript
if (typeof SharedArrayBuffer === 'undefined') {
  console.error('SharedArrayBuffer is not available. Check COOP/COEP headers.');
  // Multi-threading will fail, player may not initialize
}
```

### Check Codec Support

```javascript
const mimeType = 'video/mp4; codecs="avc1.42E01E"';
if (!MediaSource.isTypeSupported(mimeType)) {
  console.warn(`Codec not supported: ${mimeType}`);
}
```

### Complete Compatibility Check

```javascript
function checkPlayerCompatibility() {
  const checks = {
    mse: !!window.MediaSource,
    sharedArrayBuffer: typeof SharedArrayBuffer !== 'undefined',
    webAssembly: typeof WebAssembly !== 'undefined',
    https: location.protocol === 'https:' || location.hostname === 'localhost'
  };
  
  const allPassed = Object.values(checks).every(Boolean);
  
  if (!allPassed) {
    console.error('Compatibility check failed:', checks);
    return false;
  }
  
  console.log('All compatibility checks passed');
  return true;
}

// Run check before initializing player
if (!checkPlayerCompatibility()) {
  // Show error to user or use fallback
}
```

## Important Notes

### Browser-Only Platform

This player **only works in browsers** due to its dependency on Media Source Extensions (MSE). It cannot be used in:
- Node.js or server-side JavaScript
- Native mobile apps (without WebView)
- Desktop applications (without browser engine)

See [MSE Dependency](../architecture/mse-dependency.md) for detailed explanation.

### SharedArrayBuffer Security

The COOP/COEP headers are required by browsers for security reasons (Spectre/Meltdown mitigations). These headers:
- Enable SharedArrayBuffer support
- Are mandatory for multi-threading
- Must be set correctly or player will fail

## Next Steps

- [Quick Start Guide](quick-start.md) - Create your first player instance
- [Setup & Configuration](../api/setup.md) - Configure the player
- [MSE Dependency](../architecture/mse-dependency.md) - Understand browser-only constraints
