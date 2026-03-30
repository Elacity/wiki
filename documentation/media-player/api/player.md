# Player API

This page covers the `create()` factory function, the `ElacityMediaPlayer` instance it returns, and the supporting types used during playback.

## `create()`

Factory function that creates a new player instance bound to a specific NFT and a DOM media element. Each call spins up its own WASM remuxer context, fetches the DASH manifest at the given URL, and prepares `MediaSource` buffers for adaptive streaming. You can create multiple independent player instances after a single `setup()` call.

### Signature

```typescript
function create(
  tokenAddr: string,
  tokenId: string,
  videoElement: HTMLMediaElement | HTMLVideoElement,
  src: string,
  options?: PlaybackOptions,
): Promise<ElacityMediaPlayer>
```

### Parameters

| Parameter | Type | Description |
|---|---|---|
| `tokenAddr` | `string` | NFT contract address that owns the media |
| `tokenId` | `string` | Token ID within the contract |
| `videoElement` | `HTMLMediaElement` | The `<video>` or `<audio>` DOM element used for rendering |
| `src` | `string` | URL of the MPEG-DASH manifest (`.mpd` file) |
| `options` | `PlaybackOptions` | Optional playback configuration (see below) |

### Returns

A `Promise<ElacityMediaPlayer>` that resolves once the WASM runtime is ready and the manifest has been parsed.

### Example

```javascript
import { create } from '@elacity-js/media-player';

const video = document.querySelector('video');

const player = await create(
  '0xAbC123...', // NFT contract address
  '42',          // token ID
  video,
  'https://cdn.example.com/content/manifest.mpd',
);

await player.play();
```

---

## `ElacityMediaPlayer`

The main player interface returned by `create()`. It provides methods to control playback, manage DRM certificates, and inspect media metadata. It extends the standard [`EventTarget`](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget) interface, so you attach listeners with `addEventListener` / `removeEventListener` as you would with any DOM element. See the [Events](events.md) page for the full list of emitted events.

```typescript
interface ElacityMediaPlayer extends EventTarget {
  metadata: PlayerMetadata;
  certificate?: PlayerCertificate;
  currentTime: number;

  play(options?: PlaybackOptions): Promise<PlayerSignal>;
  pause(): void;
  setCertificate(certificate: PlayerCertificate): void;
}
```

### Properties

#### `metadata`

Read-only media information populated after the WASM remuxer has parsed the manifest.

```typescript
interface PlayerMetadata {
  /** MIME type + codec strings for each stream (e.g. `['video/mp4; codecs="avc1.64001f"']`) */
  mime_codec: string[];
  /** Total duration in seconds */
  duration: number;
}
```

```javascript
console.log(player.metadata.duration);    // 183.5
console.log(player.metadata.mime_codec);  // ['video/mp4; codecs="avc1.64001f"', 'audio/mp4; codecs="mp4a.40.2"']
```

#### `certificate`

An optional pre-authentication certificate. When set before calling `play()`, the player skips the wallet-signature step and uses this certificate for license acquisition instead.

```typescript
interface PlayerCertificate {
  /** Ethereum address of the signer */
  signer: string;
  /** EIP-712 signature */
  signature: string;
}
```

#### `currentTime`

Current playback position in seconds. Readable and writable – assigning a value triggers a seek.

```javascript
// Read
console.log(player.currentTime); // 42.3

// Seek
player.currentTime = 120; // jump to 2 minutes
```

### Methods

#### `play(options?)`

Starts or resumes playback. If a DRM license is required and no `certificate` is available, the player will request a wallet signature (emitting `sign_request` and then `certificate` events).

```typescript
play(options?: PlaybackOptions): Promise<PlayerSignal>
```

The returned `PlayerSignal` indicates the outcome of the play request:

```typescript
type PlayerSignal<T = unknown> = {
  code: number;
  value: number;
  data: T;
};
```

**Signal codes** (exported as `PLAYER_SIGNAL`):

| Constant | Code | Meaning |
|---|---|---|
| `SIG_STREAM_FETCH_START` | `11` | Stream fetch has begun |
| `SIG_STREAM_END` | `10` | Stream reached the end |
| `SIG_LICENSE_OK` | `20` | License acquired successfully |
| `SIG_LICENSE_REQUIRED` | `21` | License is required before playback can start |

**Example – basic playback:**

```javascript
const signal = await player.play();
console.log('play resolved with signal code:', signal.code);
```

**Example – with certificate (skip wallet signature):**

```javascript
await player.play({
  certificate: {
    signer: '0x...',
    signature: '0x...',
  },
});
```

**Example – with pre-play hook:**

```javascript
await player.play({
  onBeforePlay: async (instance) => {
    // e.g. log analytics, request certificate from a backend, etc.
    const cert = await fetchCertificateFromServer(instance);
    instance.setCertificate(cert);
  },
});
```

#### `pause()`

Pauses playback. Emits a `paused` event (via the `statechanged` pipeline).

```javascript
player.pause();
```

#### `setCertificate(certificate)`

Sets the certificate on the player instance for pre-authentication. Calling this before `play()` prevents the player from requesting a wallet signature.

```typescript
setCertificate(certificate: PlayerCertificate): void
```

```javascript
player.setCertificate({
  signer: '0xAbC...',
  signature: '0x1234...',
});

await player.play(); // no wallet popup
```

---

## `PlaybackOptions`

Configuration object accepted by both `create()` and `player.play()`. When passed to `create()`, the options act as defaults for every subsequent `play()` call on that instance. When passed directly to `play()`, they apply only to that playback session and can override both the create-time defaults and the global DRM setup.

```typescript
interface PlaybackOptions {
  appendMode?: string;
  segleng?: number;
  fromts?: number;
  logLevel?: number;
  certificate?: PlayerCertificate;
  aspectRatio?: string;
  thumbnail?: string;
  onBeforePlay?: (instance: ElacityMediaPlayer) => Promise<void>;
  handlebars?: {
    title?: string;
    author?: string;
    thumbnail?: string;
  };
  skinVideoHTML?: string;
  skinAudioHTML?: string;
  drmSystem?: Partial<Record<DrmSystemType, DrmSystemParameters>>;
}
```

| Option | Type | Default | Description |
|---|---|---|---|
| `appendMode` | `string` | `'segments'` | SourceBuffer append mode |
| `segleng` | `number` | `2` | Segment length (in seconds) |
| `fromts` | `number` | `0` | Start playback at this timestamp (seconds) |
| `logLevel` | `number` | `24` | WASM-side log verbosity (0 = silent, higher = more verbose) |
| `certificate` | `PlayerCertificate` | – | Pre-authentication certificate |
| `aspectRatio` | `string` | `'16/9'` | CSS aspect-ratio for the video element |
| `thumbnail` | `string` | – | Poster / thumbnail URL |
| `onBeforePlay` | `(instance) => Promise<void>` | – | Async callback invoked just before playback begins |
| `handlebars` | `object` | – | Template variables injected into the player skin |
| `skinVideoHTML` | `string` | – | Custom HTML template for the video player skin |
| `skinAudioHTML` | `string` | – | Custom HTML template for the audio player skin |
| `drmSystem` | `Partial<Record<DrmSystemType, DrmSystemParameters>>` | – | Per-playback DRM override (see [Setup](setup.md#drmsystem-optional)) |

### Per-playback DRM override

The `drmSystem` option lets you override the default DRM configuration for a single `play()` call without affecting the global setup:

```javascript
await player.play({
  drmSystem: {
    'cenc:web3-drm-v1': { priority: 0 }, // force Web3 DRM for this playback
  },
});
```

---

## `PLAYER_STATE`

Enumeration of all states a player instance can be in throughout its lifecycle. The current state is communicated via the [`statechanged`](events.md#statechanged) event, which fires on every transition and carries both the previous and the new state. The JS layer also re-emits high-level convenience events (`ready`, `playing`, `paused`, `ended`) derived from these transitions.

```javascript
import { PLAYER_STATE } from '@elacity-js/media-player';
```

| Constant | Value | Description |
|---|---|---|
| `LOADING` | `'loading'` | Runtime is initialising and loading resources |
| `LOADED` | `'loaded'` | Media and buffers are fully loaded |
| `INITED` | `'inited'` | Playback initiated, awaiting metadata |
| `READY` | `'ready'` | Metadata loaded, ready to play |
| `PLAYING` | `'playing'` | Media is actively playing |
| `STREAMING` | `'streaming'` | Actively streaming from the network source |
| `PAUSED` | `'paused'` | Playback paused |
| `STOPPED` | `'stopped'` | Playback stopped (may require re-initialisation) |
| `ENDED` | `'ended'` | Reached the end of the media |

---

## `ffmpegUtils()`

Utility function that exposes MIME-type detection backed by the WASM runtime's internal ffprobe logic. It operates on the Emscripten virtual filesystem, so you can write a file into it and then probe its MIME type without any network round-trip.

```typescript
function ffmpegUtils(): Promise<{
  ffmime: (filePath?: string) => string;
  ffmimeFS: any;
}>
```

| Return field | Description |
|---|---|
| `ffmime(filePath?)` | Returns the MIME type string for a given file path in the virtual filesystem |
| `ffmimeFS` | Reference to the Emscripten virtual filesystem – allows writing files before probing |

```javascript
import { ffmpegUtils } from '@elacity-js/media-player';

const { ffmime, ffmimeFS } = await ffmpegUtils();
const mime = ffmime('/input.mp4'); // 'video/mp4'
```

---

## Full Integration Example

```javascript
import { setup, create, PLAYER_STATE } from '@elacity-js/media-player';
import { BrowserProvider } from 'ethers';

const provider = new BrowserProvider(window.ethereum);
await setup({ provider });

const video = document.getElementById('player');
const player = await create(
  '0xAbC123...',
  '42',
  video,
  'https://cdn.example.com/manifest.mpd',
);

// Listen for state transitions
player.addEventListener('statechanged', (e) => {
  const { prevState, state } = e.detail;
  console.log(`${prevState} → ${state}`);
});

// Handle errors
player.addEventListener('error', (e) => {
  console.error('Playback error:', e.detail.error);
});

// Cache certificates for reuse
player.addEventListener('certificate', (e) => {
  sessionStorage.setItem('drm-cert', JSON.stringify(e.detail));
});

// Start playback
await player.play();
```

## Related

- [Setup & Configuration](setup.md) – `setup()`, `setProvider()`, DRM configuration
- [Events](events.md) – event reference and handling patterns
