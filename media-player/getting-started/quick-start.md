# Quick Start

Get your first Elacity Media Player instance running in minutes.

**⚠️ Prerequisites**: This player only works in browsers with Media Source Extensions (MSE) support. Ensure your server has COOP/COEP headers configured for SharedArrayBuffer support. See [Installation Guide](installation.md) for setup.

## Basic Setup

### 1. Import the Player

```javascript
import { setup, create, setProvider } from '@elacity-js/media-player';
```

### 2. Initialize the Player

```javascript
// Setup (one-time initialization)
await setup({
  provider: window.ethereum, // Web3 provider
  drmSystem: {
    'cenc:lit-drm-v1': { priority: 0 },
    'cenc:web3-drm-v1': { priority: 1 }
  }
});
```

### 3. Create a Player Instance

```javascript
const videoElement = document.querySelector('video');
const manifestUrl = 'https://example.com/manifest.mpd';
const tokenAddress = '0x...'; // NFT contract address
const tokenId = '123'; // NFT token ID

const player = await create(
  tokenAddress,
  tokenId,
  videoElement,
  manifestUrl
);
```

### 4. Play Media

```javascript
await player.play({
  certificate: {
    signer: '0x...',
    signature: '0x...'
  }
});
```

## Complete Example

```html
<!DOCTYPE html>
<html>
<head>
  <title>Elacity Media Player</title>
</head>
<body>
  <video id="video" controls></video>
  
  <script type="module">
    import { setup, create } from '@elacity-js/media-player';
    
    async function initPlayer() {
      // 1. Setup player
      await setup({
        provider: window.ethereum,
        drmSystem: {
          'cenc:lit-drm-v1': { priority: 0 }
        }
      });
      
      // 2. Request wallet connection
      await window.ethereum.request({ method: 'eth_requestAccounts' });
      
      // 3. Create player
      const videoElement = document.getElementById('video');
      const player = await create(
        '0x1234...', // NFT contract address
        '1',         // Token ID
        videoElement,
        'https://example.com/manifest.mpd'
      );
      
      // 4. Listen for events
      player.addEventListener('ready', () => {
        console.log('Player ready!');
      });
      
      player.addEventListener('error', (e) => {
        console.error('Player error:', e.detail);
      });
      
      // 5. Play
      await player.play();
    }
    
    initPlayer().catch(console.error);
  </script>
</body>
</html>
```

## React Example

```jsx
import { useEffect, useRef, useState } from 'react';
import { setup, create } from '@elacity-js/media-player';

function MediaPlayer({ tokenAddress, tokenId, manifestUrl }) {
  const videoRef = useRef(null);
  const [player, setPlayer] = useState(null);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let playerInstance = null;
    
    async function init() {
      try {
        // Setup
        await setup({
          provider: window.ethereum,
          drmSystem: {
            'cenc:lit-drm-v1': { priority: 0 }
          }
        });
        
        // Create player
        playerInstance = await create(
          tokenAddress,
          tokenId,
          videoRef.current,
          manifestUrl
        );
        
        setPlayer(playerInstance);
        
        // Event listeners
        playerInstance.addEventListener('ready', () => {
          console.log('Ready');
        });
        
        playerInstance.addEventListener('error', (e) => {
          setError(e.detail);
        });
      } catch (err) {
        setError(err.message);
      }
    }
    
    if (videoRef.current && tokenAddress && tokenId && manifestUrl) {
      init();
    }
    
    return () => {
      if (playerInstance) {
        // Cleanup handled by player
      }
    };
  }, [tokenAddress, tokenId, manifestUrl]);
  
  const handlePlay = async () => {
    if (player) {
      try {
        await player.play();
      } catch (err) {
        setError(err.message);
      }
    }
  };
  
  return (
    <div>
      <video ref={videoRef} controls />
      {error && <div>Error: {error}</div>}
      <button onClick={handlePlay}>Play</button>
    </div>
  );
}
```

## Vue Example

```vue
<template>
  <div>
    <video ref="videoElement" controls></video>
    <button @click="play" :disabled="!player">Play</button>
    <div v-if="error">Error: {{ error }}</div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { setup, create } from '@elacity-js/media-player';

const props = defineProps({
  tokenAddress: String,
  tokenId: String,
  manifestUrl: String
});

const videoElement = ref(null);
const player = ref(null);
const error = ref(null);

onMounted(async () => {
  try {
    await setup({
      provider: window.ethereum,
      drmSystem: {
        'cenc:lit-drm-v1': { priority: 0 }
      }
    });
    
    player.value = await create(
      props.tokenAddress,
      props.tokenId,
      videoElement.value,
      props.manifestUrl
    );
    
    player.value.addEventListener('error', (e) => {
      error.value = e.detail;
    });
  } catch (err) {
    error.value = err.message;
  }
});

const play = async () => {
  if (player.value) {
    try {
      await player.value.play();
    } catch (err) {
      error.value = err.message;
    }
  }
};
</script>
```

## Important Notes

### Browser-Only Platform

This player requires:
- ✅ Browser environment (Chrome, Firefox, Safari, Edge)
- ✅ Media Source Extensions (MSE) support
- ✅ SharedArrayBuffer (requires COOP/COEP headers)
- ❌ Cannot run in Node.js or server-side environments

See [MSE Dependency](../architecture/mse-dependency.md) for detailed explanation.

## Next Steps

- [Player API Reference](../api/player.md) - Full API documentation
- [Events](../api/events.md) - Event handling guide
- [DRM Systems](../architecture/drm-systems.md) - Understanding DRM systems
- [MSE Dependency](../architecture/mse-dependency.md) - Browser-only architecture explained
