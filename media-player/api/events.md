# Events

The `ElacityMediaPlayer` extends [`EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget), so all standard methods — `addEventListener`, `removeEventListener`, `dispatchEvent` — work as expected.

Events fall into two layers:

1. **Low-level events** — `statechanged`, `metadata`, `signal_received` — come directly from the WASM remuxer and carry raw information.
2. **High-level events** — `playing`, `paused`, `ended`, `ready`, `error`, etc. — are derived from the low-level events on the JavaScript side to provide a more ergonomic integration API.

You can listen to either layer depending on your needs.

---

## Low-Level Events

### `statechanged`

Emitted every time the player transitions between [states](player.md#player_state). This is the primitive event dispatched by the remuxer; the JS layer interprets it and re-emits the corresponding high-level event.

**Detail:**

```typescript
{
  prevState: string; // previous PLAYER_STATE value
  state: string;     // new PLAYER_STATE value
}
```

**Example:**

```javascript
player.addEventListener('statechanged', (e) => {
  const { prevState, state } = e.detail;
  console.log(`${prevState} → ${state}`);

  if (state === 'ready') {
    // equivalent to listening for the high-level 'ready' event
  }
});
```

### `metadata`

Emitted once when the remuxer has finished parsing the DASH manifest and resolved stream information.

**Detail:**

```typescript
{
  streams: Array<{
    id: string;
    codec: string;
    mime_type: string;
    bitrate: number;
    width?: number;   // video streams only
    height?: number;  // video streams only
    selected: number;
  }>;
  duration: number;
  stream_mapping_initial_values: number[];
  playback_ctx: number;
  seek_state: number;
  await_ptr: number;
  stream_mapping_address: number;
  stream_mapping_count: number;
}
```

**Example:**

```javascript
player.addEventListener('metadata', (e) => {
  const { streams, duration } = e.detail;
  console.log(`Duration: ${duration}s, streams: ${streams.length}`);

  streams.forEach((s) => {
    console.log(`  ${s.mime_type} — ${s.codec} @ ${s.bitrate} bps`);
  });
});
```

### `signal_received`

Emitted whenever the remuxer sends a signal to the JS layer. See [PlayerSignal codes](player.md#playsignal-codes) for possible values.

**Detail:**

```typescript
{
  code: number;
  value: number;
  data: unknown;
}
```

**Example:**

```javascript
import { PLAYER_SIGNAL } from '@elacity-js/media-player';

player.addEventListener('signal_received', (e) => {
  if (e.detail.code === PLAYER_SIGNAL.SIG_LICENSE_REQUIRED) {
    console.log('License required — wallet signature will be requested');
  }
});
```

---

## High-Level Events

These are derived from the low-level events and provide a cleaner surface for common use cases.

### Playback Events

#### `ready`

The player has loaded metadata and is ready to accept a `play()` call.

```javascript
player.addEventListener('ready', () => {
  playButton.disabled = false;
});
```

#### `playing`

Playback has started or resumed.

```javascript
player.addEventListener('playing', () => {
  playButton.textContent = 'Pause';
});
```

#### `paused`

Playback has been paused (either programmatically or by the user).

```javascript
player.addEventListener('paused', () => {
  playButton.textContent = 'Play';
});
```

#### `ended`

Playback has reached the end of the media.

```javascript
player.addEventListener('ended', () => {
  playButton.textContent = 'Replay';
  console.log('Playback finished');
});
```

#### `stop`

Playback has been stopped entirely. Unlike `paused`, a stopped player may need re-initialisation to resume.

```javascript
player.addEventListener('stop', () => {
  console.log('Player stopped');
});
```

### DRM Events

#### `sign_request`

Emitted just before the player asks the Web3 provider for an EIP-712 signature. Use this to show a UI hint so the user knows a wallet popup is coming.

```javascript
player.addEventListener('sign_request', () => {
  showNotification('Please approve the signature request in your wallet');
});
```

#### `certificate`

Emitted after a signature is successfully obtained. The detail contains everything needed to cache and reuse the certificate, avoiding repeated wallet popups.

**Detail:**

```typescript
{
  signature: string;   // EIP-712 signature
  signer: string;      // signer address
  entity: {
    ledger: string;    // NFT contract address
    tokenId: string;   // token ID
  };
}
```

**Example — cache and reuse:**

```javascript
player.addEventListener('certificate', (e) => {
  const { signature, signer, entity } = e.detail;

  // Persist for the session
  sessionStorage.setItem(
    `cert:${entity.ledger}:${entity.tokenId}`,
    JSON.stringify({ signature, signer }),
  );
});

// Later, on a new player instance for the same NFT
const cached = sessionStorage.getItem(`cert:${tokenAddr}:${tokenId}`);
if (cached) {
  player.setCertificate(JSON.parse(cached));
}
await player.play(); // no wallet popup
```

#### `sign_error`

Emitted when the signature request fails (user rejection, timeout, etc.).

**Detail:**

```typescript
{
  error: Error;
}
```

Common error codes (from the EIP-1193 provider):

| Code | Meaning |
|---|---|
| `4001` | User rejected the request |
| `-32002` | A request is already pending |

**Example:**

```javascript
player.addEventListener('sign_error', (e) => {
  const { error } = e.detail;

  if (error.code === 4001) {
    showNotification('Signature rejected — playback cannot start without authorisation');
  } else if (error.code === -32002) {
    showNotification('A signature request is already pending in your wallet');
  } else {
    console.error('Unexpected signing error:', error);
  }
});
```

### Error Events

#### `error`

Emitted when a playback or DRM error occurs. The detail wraps the raw information from the remuxer into a standard JavaScript `Error`.

**Detail:**

```typescript
{
  error: Error;
}
```

**Example:**

```javascript
player.addEventListener('error', (e) => {
  const { error } = e.detail;
  console.error('Player error:', error.message);
});
```

---

## Event Handling Patterns

### Removing listeners

Always keep a reference if you plan to remove the listener later:

```javascript
const onReady = () => console.log('ready');
player.addEventListener('ready', onReady);

// Cleanup
player.removeEventListener('ready', onReady);
```

### One-time listeners

Use the `{ once: true }` option for events you only need to handle once:

```javascript
player.addEventListener('ready', () => {
  console.log('First ready — this handler is now removed');
}, { once: true });
```

### Tracking state with a single handler

If you prefer a centralised approach, use `statechanged` instead of individual high-level events:

```javascript
let playerState = 'loading';

player.addEventListener('statechanged', (e) => {
  playerState = e.detail.state;
  updateUI(playerState);
});
```

---

## React Integration

```jsx
import { useEffect, useState, useRef } from 'react';

function usePlayer(player) {
  const [state, setState] = useState('loading');
  const [error, setError] = useState(null);

  useEffect(() => {
    if (!player) return;

    const onStateChanged = (e) => setState(e.detail.state);
    const onError = (e) => {
      setError(e.detail.error);
      setState('error');
    };

    player.addEventListener('statechanged', onStateChanged);
    player.addEventListener('error', onError);

    return () => {
      player.removeEventListener('statechanged', onStateChanged);
      player.removeEventListener('error', onError);
    };
  }, [player]);

  return { state, error };
}
```

---

## Error Recovery

### Retry on license failure

```javascript
let retries = 0;
const MAX_RETRIES = 3;

player.addEventListener('error', async (e) => {
  if (retries < MAX_RETRIES) {
    retries++;
    console.log(`Retrying playback (${retries}/${MAX_RETRIES})...`);
    await new Promise((r) => setTimeout(r, 1000 * retries)); // exponential-ish backoff
    await player.play();
  } else {
    console.error('Max retries reached:', e.detail.error);
  }
});
```

### Fallback DRM system

```javascript
player.addEventListener('error', async (e) => {
  // If the primary DRM system failed, try a different one
  await player.play({
    drmSystem: {
      'cenc:web3-drm-v1': { priority: 0 },
    },
  });
});
```

---

## Event Summary

| Event | Layer | Detail | When |
|---|---|---|---|
| `statechanged` | low | `{ prevState, state }` | Every state transition |
| `metadata` | low | Stream info, duration, codec details | Manifest parsed |
| `signal_received` | low | `{ code, value, data }` | Remuxer signal |
| `ready` | high | – | Player ready to play |
| `playing` | high | – | Playback started / resumed |
| `paused` | high | – | Playback paused |
| `ended` | high | – | Media finished |
| `stop` | high | – | Playback stopped |
| `sign_request` | high | – | Wallet signature requested |
| `certificate` | high | `{ signature, signer, entity }` | Certificate obtained |
| `sign_error` | high | `{ error }` | Signature failed |
| `error` | high | `{ error }` | Playback / DRM error |

## Related

- [Player API](player.md) – player instance methods and properties
- [Setup & Configuration](setup.md) – initial setup and provider injection
- [DRM Systems](../architecture/drm-systems.md) – DRM architecture details
