# Player API Reference

Complete API reference for the Elacity Media Player.

## `create()`

Creates a new player instance.

### Signature

```typescript
function create(
  tokenAddress: string,
  tokenId: string,
  videoElement: HTMLVideoElement | HTMLMediaElement,
  src: string,
  options?: PlaybackOptions
): Promise<ElacityMediaPlayer>
```

### Parameters

- **`tokenAddress`** (string): NFT contract address
- **`tokenId`** (string): NFT token ID
- **`videoElement`** (HTMLVideoElement): Video element for playback
- **`src`** (string): DASH manifest URL (`.mpd` file)
- **`options`** (PlaybackOptions, optional): Playback configuration

### Returns

Promise resolving to `ElacityMediaPlayer` instance.

### Example

```javascript
const player = await create(
  '0x1234...',
  '1',
  document.querySelector('video'),
  'https://example.com/manifest.mpd'
);
```

## `ElacityMediaPlayer`

Main player class extending `EventTarget`.

### Properties

#### `currentTime: number`
Current playback time in seconds. Can be set to seek.

```javascript
player.currentTime = 120; // Seek to 2 minutes
const time = player.currentTime;
```

#### `metadata: PlayerMetadata`
Media metadata object containing:
- `mime_codec: string[]`: MIME types and codecs
- `duration: number`: Media duration in seconds

```javascript
console.log(player.metadata.duration);
console.log(player.metadata.mime_codec);
```

#### `certificate?: PlayerCertificate`
Optional certificate for pre-authentication:
- `signer: string`: Signer address
- `signature: string`: EIP-712 signature

```javascript
player.setCertificate({
  signer: '0x...',
  signature: '0x...'
});
```

### Methods

#### `play(options?: PlaybackOptions): Promise<PlayerSignal>`

Starts or resumes playback.

**Parameters:**
- `options` (PlaybackOptions, optional): Playback options

**Returns:** Promise resolving to `PlayerSignal`

**Example:**

```javascript
await player.play({
  certificate: {
    signer: '0x...',
    signature: '0x...'
  },
  onBeforePlay: async (instance) => {
    // Custom logic before playback
  }
});
```

#### `pause(): void`

Pauses playback.

```javascript
player.pause();
```

#### `setCertificate(certificate: PlayerCertificate): void`

Sets certificate for pre-authentication.

```javascript
player.setCertificate({
  signer: '0x...',
  signature: '0x...'
});
```

### Events

The player extends `EventTarget` and emits the following events:

#### `ready`
Emitted when player is ready for playback.

```javascript
player.addEventListener('ready', () => {
  console.log('Player ready');
});
```

#### `playing`
Emitted when playback starts.

```javascript
player.addEventListener('playing', () => {
  console.log('Playing');
});
```

#### `paused`
Emitted when playback is paused.

```javascript
player.addEventListener('paused', () => {
  console.log('Paused');
});
```

#### `ended`
Emitted when playback ends.

```javascript
player.addEventListener('ended', () => {
  console.log('Playback ended');
});
```

#### `error`
Emitted when an error occurs.

```javascript
player.addEventListener('error', (e) => {
  console.error('Error:', e.detail);
});
```

#### `certificate`
Emitted when a certificate is generated (for caching).

```javascript
player.addEventListener('certificate', (e) => {
  const { signature, signer, entity } = e.detail;
  // Cache certificate
});
```

#### `sign_request`
Emitted when signature is requested from wallet.

```javascript
player.addEventListener('sign_request', () => {
  console.log('Please sign the message');
});
```

#### `sign_error`
Emitted when signature request fails.

```javascript
player.addEventListener('sign_error', (e) => {
  console.error('Signature error:', e.detail.error);
});
```

## `PlaybackOptions`

Configuration options for playback.

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
  drmSystem?: Partial<DrmSystemType, DrmSystemParameters>;
}
```

### Options

- **`appendMode`**: SourceBuffer append mode
- **`segleng`**: Segment length
- **`fromts`**: Start timestamp
- **`logLevel`**: Logging level (0-4)
- **`certificate`**: Pre-authentication certificate
- **`aspectRatio`**: Video aspect ratio
- **`thumbnail`**: Thumbnail URL
- **`onBeforePlay`**: Callback before playback starts
- **`handlebars`**: Template variables for UI
- **`skinVideoHTML`**: Custom video skin HTML
- **`skinAudioHTML`**: Custom audio skin HTML
- **`drmSystem`**: DRM system override

## `PlayerSignal`

Signal object emitted by player.

```typescript
interface PlayerSignal<T = any> {
  code: number;
  value: number;
  data: T;
}
```

### Signal Codes

- `10`: Stream ended (`SIG_STREAM_END`)
- `11`: Stream fetch started (`SIG_STREAM_FETCH_START`)
- `20`: License acquired (`SIG_LICENSE_OK`)
- `21`: License required (`SIG_LICENSE_REQUIRED`)

## `PLAYER_STATE`

Player state constants.

```javascript
PLAYER_STATE.LOADING   // Initializing
PLAYER_STATE.LOADED    // Media loaded
PLAYER_STATE.INITED    // Initialized
PLAYER_STATE.READY     // Ready for playback
PLAYER_STATE.PLAYING   // Currently playing
PLAYER_STATE.STREAMING // Streaming content
PLAYER_STATE.PAUSED    // Paused
PLAYER_STATE.STOPPED   // Stopped
PLAYER_STATE.ENDED     // Playback ended
```

## Usage Examples

### Basic Playback

```javascript
const player = await create(
  tokenAddress,
  tokenId,
  videoElement,
  manifestUrl
);

await player.play();
```

### With Certificate

```javascript
await player.play({
  certificate: {
    signer: '0x...',
    signature: '0x...'
  }
});
```

### Event Handling

```javascript
player.addEventListener('ready', () => {
  console.log('Ready');
});

player.addEventListener('error', (e) => {
  console.error('Error:', e.detail);
});

await player.play();
```

### Seeking

```javascript
player.currentTime = 120; // Seek to 2 minutes
```

## Related Documentation

- [Setup API](setup.md) - Player setup and configuration
- [Events](events.md) - Event handling guide
- [Types](types.md) - TypeScript type definitions
