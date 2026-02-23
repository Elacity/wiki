# Architecture Overview

Understanding the internal architecture of the Elacity Media Player helps you make informed decisions about integration and customization.

## Two-Layer Architecture

The player is built on a two-layer architecture with a **critical dependency on Media Source Extensions (MSE)**:

```
┌─────────────────────────────────────────┐
│   JavaScript Wrapper (Browser API)      │
│   - Player Controller                   │
│   - MSE Integration (CRITICAL)          │
│   - License Acquisition (Adapter)       │
│   - Event Management                    │
└─────────────────┬───────────────────────┘
                  │
                  │ WebAssembly Interface
                  │
┌─────────────────▼───────────────────────┐
│   C Core (WASM)                         │
│   - FFmpeg Integration                  │
│   - DASH Parser                         │
│   - Media Decoding                      │
│   - DRM Protocol - `CEK` provider       │
│   - Remuxing to browser format          │
└─────────────────┬───────────────────────┘
                  │
                  │ Decoded media frames
                  │ (video/audio data)
                  ▼
┌─────────────────────────────────────────┐
│   Media Source Extensions (MSE)         │
│   - SourceBuffer.appendBuffer()          │
│   - Browser media pipeline              │
│   - HTMLMediaElement rendering          │
│   ⚠️ BROWSER-ONLY API                    │
└─────────────────────────────────────────┘
```

**⚠️ Critical Constraint**: All decoded media data from the WASM remuxer flows through MSE's `SourceBuffer.appendBuffer()` method. This makes the player **browser-only** - it cannot function without MSE support. See [MSE Dependency](mse-dependency.md) for detailed explanation.

## JavaScript Layer

**Location**: `packages/player/src/`

### Core Components

#### `controller.js` - Player Controller
- Main player class (`ElacityMediaPlayer`)
- Manages playback state and lifecycle
- Handles Media Source Extensions (MSE)
- Coordinates between WASM and browser APIs

#### `loader.js` - Runtime Loader
- Loads WebAssembly module
- Initializes WASM runtime
- Sets up bridge functions between JS and WASM
- Manages crypto protocol module loading

#### `license/` - License Acquisition
- `index.js`: License request orchestrator
- `lit.js`: Lit Protocol integration
- `web3based.js`: Web3 DRM integration
- Handles multiple DRM systems with priority fallback

#### `buffer.js` - Buffer Management
- `BufferQueue`: Manages media segment queues
- Handles SourceBuffer operations
- Implements buffer duration limits
- Manages memory cleanup

#### `helpers.js` - Utilities
- MIME type detection
- Codec string generation
- Buffer duration calculations

## C Core Layer

**Location**: `src/`

### Core Modules

#### `play.c` - Main Entry Point
- Initializes FFmpeg contexts
- Orchestrates playback pipeline
- Manages segment processing loop
- Handles seeking and timing

#### `media/` - Media Processing
- `player.c/h`: Core playback engine
- `parser.c/h`: DASH manifest parsing
- `dashsegment.c/h`: Segment handling
- `metadata.c/h`: Media metadata extraction

#### `protocol/` - Cryptographic Protocol
- `protocol.c/h`: Main protocol handler
- `aes.c/h`: AES encryption/decryption
- `ecdh.c/h`: ECDH key exchange
- `ecdsa.c/h`: ECDSA signature verification

#### `ioutil/` - I/O Operations
- `mse.c`: Media Source Extensions bridge
- `seek.c/h`: Seeking functionality
- `error.c/h`: Error handling
- `initfs.c/h`: File system initialization

#### `util/` - Utilities
- `logger.c/h`: Logging system
- `array.c/h`: Array utilities
- `async.c/h`: Async operations
- `player_string.c/h`: String utilities

## Data Flow

### Playback Flow

```
1. JavaScript: player.play() called
   ↓
2. JavaScript: Load DASH manifest
   ↓
3. WASM: Parse manifest, extract PSSH boxes
   ↓
4. WASM: Trigger license request callback
   ↓
5. JavaScript: License acquisition (Lit/Web3)
   ↓
6. WASM: Receive license, decrypt keys
   ↓
7. WASM: Fetch and decrypt media segments
   ↓
8. WASM: Decode media packets (via FFmpeg)
   ↓
9. WASM: Remux decoded frames to browser format
   ↓
10. WASM: Send decoded data to JavaScript
    ↓
11. JavaScript: Append to SourceBuffer via MSE API
    ⚠️ CRITICAL: This is the ONLY rendering path
    ↓
12. Browser: Render media via native MSE pipeline
```

**Key Point**: Step 11 (`SourceBuffer.appendBuffer()`) is the **only way** decoded media reaches the browser. There is no alternative rendering mechanism. This dependency on MSE makes the player browser-only.

### License Acquisition Flow

```
1. WASM extracts PSSH box from DASH manifest
   ↓
2. WASM calls __protocol__acquire_license()
   ↓
3. JavaScript LicenseRequest processes:
   - Sorts DRM systems by priority
   - Tries each system in order
   - Falls back to next if fails
   ↓
4. License processor (Lit/Web3):
   - Validates NFT ownership
   - Requests decryption keys
   - Returns license data
   ↓
5. JavaScript sends license to WASM
   ↓
6. WASM decrypts media segments
```

## WebAssembly Integration

### Exported Functions (C → JavaScript)

- `_main`: Entry point for playback
- `_ffmime`: MIME type detection utility
- `_malloc`, `_free`: Memory management

### Imported Functions (JavaScript → C)

- `__protocol__acquire_license`: License acquisition callback
- `__player__set_metadata`: Metadata callback
- `__player_receive_buffer`: Buffer data callback
- `__player__recv_signal`: Signal handling callback
- `__handle_exit_status`: Exit status handler

### Bridge Functions

The player uses Emscripten's `cwrap` and `ccall` for function bridging:

```javascript
// C function: int process_segment(char* data, int length)
const processSegment = runtime.cwrap('process_segment', 'number', ['string', 'number']);
```

## Threading Model

The player uses Web Workers and **SharedArrayBuffer** for multi-threading:

- **Main Thread**: JavaScript API, UI, event handling, MSE operations
- **Worker Thread**: WASM execution, media processing
- **Communication**: MessagePort for thread communication

### Thread Safety

- **SharedArrayBuffer** for shared memory between threads
- **Atomics API** for synchronization
- **MessagePort** for async communication

### SharedArrayBuffer Requirement

**⚠️ Critical**: Multi-threading requires `SharedArrayBuffer`, which has strict security requirements:

- **Required HTTP Headers**:
  ```
  Cross-Origin-Opener-Policy: same-origin
  Cross-Origin-Embedder-Policy: require-corp
  ```
- **Without these headers**: `SharedArrayBuffer` is `undefined`, multi-threading fails
- **Impact**: Player may fail to initialize or operate in degraded mode
- **Reference**: See [Installation Guide](../getting-started/installation.md) for server configuration

This requirement, combined with MSE dependency, further restricts the player to properly configured browser environments.

## Memory Management

### WASM Memory

- Initial: 32MB (`INITIAL_MEMORY`)
- Maximum: 4GB (`MAXIMUM_MEMORY`, configurable)
- Growth: Enabled (`ALLOW_MEMORY_GROWTH`)

### Buffer Management

- SourceBuffer duration limits prevent memory overflow
- Automatic cleanup of played segments
- Manual flushing for large media files

### Memory Limits

- **iOS**: 1GB maximum (browser limitation)
- **Desktop**: Up to 4GB (configurable)

## Error Handling

### Error Propagation

```
WASM Error → JavaScript Callback → Player Event → User Handler
```

### Error Types

- **License Errors**: DRM acquisition failures
- **Network Errors**: Segment fetch failures
- **Decoding Errors**: Media processing failures
- **Buffer Errors**: MSE/SourceBuffer issues

## Performance Considerations

### Optimization Strategies

1. **Segment Size**: Balance between latency and efficiency
2. **Buffer Duration**: Prevent memory overflow
3. **Threading**: Parallel processing where possible
4. **Codec Selection**: Choose efficient codecs (AV1, H.264)

### Bottlenecks

- **License Acquisition**: Blockchain calls add latency
- **Segment Fetching**: Network bandwidth limits
- **Decoding**: CPU-intensive operations
- **Memory**: Large media files require careful management

## Security Model

### Key Storage

- Keys never exposed to JavaScript
- Ephemeral memory for sensitive data
- Secure key exchange via ECDH

### Certificate Validation (only for [`cenc:web3-drm-v1`](./drm-systems#cenc:web3-drm-v1))

- EIP-712 signature verification
- Certificate caching for performance
- Optional pre-authentication

## Related Documentation

- [MSE Dependency](mse-dependency.md) - **Critical**: Browser-only architecture explained
- [DRM Systems](drm-systems.md) - Understanding DRM integration
- [Media Formats](media-formats.md) - Supported formats and codecs
- [Player API](../api/player.md) - JavaScript API reference
