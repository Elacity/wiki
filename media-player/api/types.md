# TypeScript Types

Complete TypeScript type definitions for the Elacity Media Player.

## Core Types

### `ElacityMediaPlayer`

Main player interface extending `EventTarget`.

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

### `PlayerMetadata`

Media metadata object.

```typescript
interface PlayerMetadata {
  mime_codec: string[];
  duration: number;
}
```

### `PlayerCertificate`

Certificate for pre-authentication.

```typescript
interface PlayerCertificate {
  signer: string;
  signature: string;
}
```

## Configuration Types

### `PlaybackOptions`

Options for playback configuration.

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

### `PlayerInitOptions`

Options for player setup.

```typescript
interface PlayerInitOptions<T = unknown> {
  cryptoVersion?: string;
  provider?: T;
  remote?: boolean;
  ENABLE_FS_LOGGING?: boolean;
  "go.glueCode"?: boolean;
  drmSystem?: Partial<Record<DrmSystemType, DrmSystemParameters>>;
}
```

## DRM Types

### `DrmSystemType`

Supported DRM system types.

```typescript
type DrmSystemType = 
  | 'cenc:web3-drm-v1' 
  | 'cenc:lit-drm-v1' 
  | 'cenc:lit-drm-sa-v1';
```

### `DrmSystemParameters`

DRM system configuration parameters.

```typescript
type DrmSystemParameters = {
  priority?: number;
}
```

## Event Types

### `PlayerSignal`

Signal object emitted by player.

```typescript
interface PlayerSignal<T = unknown> {
  code: number;
  value: number;
  data: T;
}
```

### Signal Codes

```typescript
const PLAYER_SIGNAL = {
  SIG_STREAM_END: 10,
  SIG_STREAM_FETCH_START: 11,
  SIG_LICENSE_OK: 20,
  SIG_LICENSE_REQUIRED: 21,
} as const;
```

### Player States

```typescript
const PLAYER_STATE = {
  LOADING: 'loading',
  LOADED: 'loaded',
  INITED: 'inited',
  READY: 'ready',
  PLAYING: 'playing',
  STREAMING: 'streaming',
  PAUSED: 'paused',
  STOPPED: 'stopped',
  ENDED: 'ended',
} as const;

type PlayerState = typeof PLAYER_STATE[keyof typeof PLAYER_STATE];
```

## Event Detail Types

### Certificate Event

```typescript
interface CertificateEventDetail {
  signature: string;
  signer: string;
  entity: {
    ledger: string;
    tokenId: string;
  };
}
```

### Error Event

```typescript
interface ErrorEventDetail {
  code?: string;
  message: string;
  details?: unknown;
}
```

### Sign Error Event

```typescript
interface SignErrorEventDetail {
  error: Error;
}
```

## Function Types

### `create()`

```typescript
function create(
  tokenAddr: string,
  tokenId: string,
  videoElement: HTMLMediaElement | HTMLVideoElement,
  src: string,
  options?: PlaybackOptions
): Promise<ElacityMediaPlayer>;
```

### `setup()`

```typescript
function setup<T = unknown>(
  options: PlayerInitOptions<T>
): Promise<void>;
```

### `setProvider()`

```typescript
function setProvider<T = unknown>(
  provider: T,
  accountOverride?: string | null
): void;
```

### `ffmpegUtils()`

```typescript
function ffmpegUtils(): Promise<{
  ffmime: (filePath?: string) => string;
  ffmimeFS: any;
}>;
```

## Utility Types

### Partial Record

```typescript
type Partial<Record<K, V>> = {
  [P in K]?: V;
}
```

## Usage Examples

### Type-Safe Player Creation

```typescript
import { create, ElacityMediaPlayer } from '@elacity-js/media-player';

const player: ElacityMediaPlayer = await create(
  '0x1234...',
  '1',
  document.querySelector('video')!,
  'https://example.com/manifest.mpd'
);
```

### Typed Event Handlers

```typescript
player.addEventListener('certificate', (e: CustomEvent<CertificateEventDetail>) => {
  const { signature, signer, entity } = e.detail;
  // Type-safe access
});

player.addEventListener('error', (e: CustomEvent<ErrorEventDetail>) => {
  const { code, message, details } = e.detail;
  // Type-safe error handling
});
```

### Typed Options

```typescript
const options: PlaybackOptions = {
  certificate: {
    signer: '0x...',
    signature: '0x...'
  },
  onBeforePlay: async (instance: ElacityMediaPlayer) => {
    // Type-safe instance
  }
};

await player.play(options);
```

### Provider Types

```typescript
import { BrowserProvider } from 'ethers';
import { setup } from '@elacity-js/media-player';

const provider = new BrowserProvider(window.ethereum);

await setup({
  provider,  // Type inferred from BrowserProvider
});
```

## Type Guards

### Check Player State

```typescript
function isReady(player: ElacityMediaPlayer): boolean {
  return player.metadata.duration > 0;
}
```

### Validate Certificate

```typescript
function isValidCertificate(
  cert: unknown
): cert is PlayerCertificate {
  return (
    typeof cert === 'object' &&
    cert !== null &&
    'signer' in cert &&
    'signature' in cert &&
    typeof (cert as PlayerCertificate).signer === 'string' &&
    typeof (cert as PlayerCertificate).signature === 'string'
  );
}
```

## Related Documentation

- [Player API](player.md) - API reference
- [Setup API](setup.md) - Setup types
- [Events](events.md) - Event types
