# Events

The Elacity Media Player extends `EventTarget` and emits events for playback state changes, errors, and DRM operations.

## Event Types

### Playback Events

#### `ready`
Emitted when the player is ready for playback.

```javascript
player.addEventListener('ready', () => {
  console.log('Player ready');
  // Safe to call player.play() now
});
```

#### `playing`
Emitted when playback starts.

```javascript
player.addEventListener('playing', () => {
  console.log('Playback started');
});
```

#### `paused`
Emitted when playback is paused.

```javascript
player.addEventListener('paused', () => {
  console.log('Playback paused');
});
```

#### `ended`
Emitted when playback reaches the end.

```javascript
player.addEventListener('ended', () => {
  console.log('Playback ended');
});
```

### DRM Events

#### `certificate`
Emitted when a certificate is generated during license acquisition. Use this to cache certificates for reuse.

**Event Detail:**
```typescript
{
  signature: string;    // EIP-712 signature
  signer: string;       // Signer address
  entity: {             // NFT entity
    ledger: string;
    tokenId: string;
  };
}
```

**Example:**

```javascript
player.addEventListener('certificate', (e) => {
  const { signature, signer, entity } = e.detail;
  
  // Cache certificate
  localStorage.setItem('certificate', JSON.stringify({
    signature,
    signer,
    entity
  }));
  
  // Reuse later
  await player.play({
    certificate: { signature, signer }
  });
});
```

#### `sign_request`
Emitted when the player requests a signature from the wallet.

```javascript
player.addEventListener('sign_request', () => {
  console.log('Please sign the message in your wallet');
  // Show UI notification
});
```

#### `sign_error`
Emitted when signature request fails.

**Event Detail:**
```typescript
{
  error: Error;  // Error object
}
```

**Example:**

```javascript
player.addEventListener('sign_error', (e) => {
  const error = e.detail.error;
  
  if (error.code === 4001) {
    console.error('User rejected signature');
  } else if (error.code === -32002) {
    console.error('Signature request already pending');
  } else {
    console.error('Signature error:', error);
  }
});
```

### Error Events

#### `error`
Emitted when a playback or DRM error occurs.

**Event Detail:**
```typescript
{
  code?: string;      // Error code
  message: string;    // Error message
  details?: any;      // Additional error details
}
```

**Error Codes:**
- `LICENSE_ERROR`: License acquisition failed
- `NETWORK_ERROR`: Network request failed
- `DECODE_ERROR`: Media decoding failed
- `BUFFER_ERROR`: SourceBuffer error

**Example:**

```javascript
player.addEventListener('error', (e) => {
  const { code, message, details } = e.detail;
  
  switch (code) {
    case 'LICENSE_ERROR':
      console.error('Failed to acquire license:', message);
      break;
    case 'NETWORK_ERROR':
      console.error('Network error:', message);
      break;
    default:
      console.error('Player error:', message, details);
  }
});
```

## Event Handling Patterns

### Single Event Handler

```javascript
player.addEventListener('ready', () => {
  console.log('Ready');
});
```

### Multiple Event Handlers

```javascript
const handleReady = () => console.log('Ready');
const handleError = (e) => console.error('Error:', e.detail);

player.addEventListener('ready', handleReady);
player.addEventListener('error', handleError);

// Remove later
player.removeEventListener('ready', handleReady);
```

### One-Time Event Handler

```javascript
player.addEventListener('ready', () => {
  console.log('Ready');
  // Handler automatically removed after first call
}, { once: true });
```

### Event Delegation

```javascript
// Handle all events with one handler
player.addEventListener('*', (e) => {
  console.log('Event:', e.type, e.detail);
});
```

## State Management

### Track Player State

```javascript
let playerState = 'loading';

player.addEventListener('ready', () => {
  playerState = 'ready';
});

player.addEventListener('playing', () => {
  playerState = 'playing';
});

player.addEventListener('paused', () => {
  playerState = 'paused';
});

player.addEventListener('ended', () => {
  playerState = 'ended';
});
```

### React Hook Example

```jsx
import { useEffect, useState } from 'react';

function usePlayerEvents(player) {
  const [state, setState] = useState('loading');
  const [error, setError] = useState(null);
  
  useEffect(() => {
    if (!player) return;
    
    const handlers = {
      ready: () => setState('ready'),
      playing: () => setState('playing'),
      paused: () => setState('paused'),
      ended: () => setState('ended'),
      error: (e) => {
        setError(e.detail);
        setState('error');
      }
    };
    
    Object.entries(handlers).forEach(([event, handler]) => {
      player.addEventListener(event, handler);
    });
    
    return () => {
      Object.entries(handlers).forEach(([event, handler]) => {
        player.removeEventListener(event, handler);
      });
    };
  }, [player]);
  
  return { state, error };
}
```

## Error Recovery

### Retry on Error

```javascript
let retryCount = 0;
const maxRetries = 3;

player.addEventListener('error', async (e) => {
  if (e.detail.code === 'LICENSE_ERROR' && retryCount < maxRetries) {
    retryCount++;
    console.log(`Retrying... (${retryCount}/${maxRetries})`);
    
    // Wait before retry
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    // Retry playback
    try {
      await player.play();
    } catch (err) {
      console.error('Retry failed:', err);
    }
  } else {
    console.error('Max retries reached or non-retryable error');
  }
});
```

### Fallback DRM System

```javascript
player.addEventListener('error', async (e) => {
  if (e.detail.code === 'LICENSE_ERROR') {
    // Try alternative DRM system
    await player.play({
      drmSystem: {
        'cenc:web3-drm-v1': { priority: 0 }  // Fallback
      }
    });
  }
});
```

## Best Practices

### 1. Always Handle Errors

```javascript
// ✅ Good
player.addEventListener('error', (e) => {
  console.error('Error:', e.detail);
});

// ❌ Bad - Errors go unhandled
await player.play();
```

### 2. Clean Up Event Listeners

```javascript
const handler = (e) => console.log(e);

player.addEventListener('ready', handler);

// Remove when done
player.removeEventListener('ready', handler);
```

### 3. Cache Certificates

```javascript
player.addEventListener('certificate', (e) => {
  // Cache for reuse
  cacheCertificate(e.detail);
});
```

### 4. User Feedback

```javascript
player.addEventListener('sign_request', () => {
  // Show UI notification
  showNotification('Please sign the message');
});

player.addEventListener('sign_error', (e) => {
  if (e.detail.error.code === 4001) {
    showNotification('Signature rejected');
  }
});
```

## Related Documentation

- [Player API](player.md) - Complete API reference
- [DRM Systems](../architecture/drm-systems.md) - DRM event handling
- [Troubleshooting](../development/troubleshooting.md) - Error debugging
