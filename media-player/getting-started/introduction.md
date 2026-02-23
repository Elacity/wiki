# Introduction

The **Elacity Media Player** (`@elacity-js/media-player`) is a WebAssembly-based media player designed to handle DRM-protected MPEG-DASH media streams **exclusively in web browsers**. 

**⚠️ CRITICAL**: This player is built entirely on [Media Source Extensions (MSE)](https://www.w3.org/TR/media-source/), which means it **only works in browser environments** and cannot be used in Node.js, server-side applications, or native mobile apps without WebView integration.

Unlike traditional DRM systems that rely on centralized license servers, the Elacity Media Player uses blockchain-based key retrieval, enabling decentralized content protection.

## What is the Elacity Media Player?

The Elacity Media Player is a production-ready media playback solution that combines:

- **WebAssembly Core**: High-performance C codebase compiled to WASM using FFmpeg libraries
- **JavaScript Wrapper**: Modern ES6+ API for easy browser integration
- **Blockchain DRM**: Decentralized license acquisition via Lit Protocol or custom Web3 DRM systems
- **MPEG-DASH Support**: Industry-standard adaptive streaming format
- **Media Source Extensions**: Native browser playback using MSE API

## Key Features

### 🎬 Media Playback
- **MPEG-DASH Streaming**: Full support for DASH manifests and segment parsing
- **Multi-Codec Support**: H.264, AV1, VP9 (video); AAC, Opus (audio)
- **Adaptive Streaming**: Buffer-based quality selection (BOLA algorithm planned)
- **Seeking & Playback Control**: Full media control API

### 🔐 Blockchain DRM
- **Lit Protocol Integration**: Native support for Lit Protocol-based encryption
- **Web3 DRM**: Custom DRM system using smart contracts
- **NFT-Gated Content**: Content access tied to NFT ownership
- **Certificate-Based Authentication**: Optional certificate pre-authentication

### ⚡ Performance
- **WebAssembly**: Near-native performance for media processing
- **Multi-threading**: Pthread support for parallel processing backed with []`SharedArrayBuffer`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer)
- **Memory Management**: Efficient buffer management and cleanup
- **Low Latency**: Optimized for streaming scenarios

## Use Cases

The Elacity Media Player is designed for:

- **NFT Marketplaces**: Play DRM-protected media tied to NFT ownership
- **Creator Platforms**: Monetize content with blockchain-based access control
- **Subscription Services**: Token-gated content access
- **Educational Platforms**: Secure content delivery with on-chain licensing

## How It Works

The player follows a flow similar to the [Encrypted Media Extension (EME)](https://www.w3.org/TR/encrypted-media) standard, but with blockchain-based key retrieval:

1. **Media Loading**: Player loads a DASH manifest (`.mpd` file)
2. **PSSH Extraction**: Extracts Protection System Specific Header (PSSH) boxes containing DRM metadata
3. **License Request**: Constructs a license request using blockchain credentials
4. **Key Retrieval**: Acquires decryption keys from blockchain (Lit Protocol or smart contracts)
5. **Decryption & Playback**: Decrypts media segments and plays via Media Source Extensions

## Architecture Overview

The player consists of two main layers:

### C Core (WASM)
- **Location**: `src/` directory
- **Purpose**: Media parsing, decoding/remuxing, and DRM protocol handling
- **Libraries**: FFmpeg (libavcodec, libavformat, libavutil), OpenSSL for all security purposes, libxml2 for DASH-MPEG format handling
- **Output**: WebAssembly module (`player.wasm`) with all the glue codes that come with it to serve as runtime `player.js` and `player.worker.js`

### JavaScript Wrapper
- **Location**: `packages/player/` directory
- **Purpose**: Browser API, MSE integration, DRM license acquisition adapter
- **Dependencies**: Lit Protocol SDK, Media Chrome (UI)
- **Output**: NPM package (`@elacity-js/media-player`)

## Browser Compatibility

**⚠️ Browser-Only Platform**: This player requires a browser environment with Media Source Extensions (MSE) support. It cannot run in Node.js, Deno, or other non-browser JavaScript runtimes.

### Supported Browsers
- ✅ Chrome/Edge 90+ (with SharedArrayBuffer support)
- ✅ Firefox 89+
- ✅ Safari 15.2+ (with limitations)

### Critical Requirements

#### Media Source Extensions (MSE)
- **Required**: The player is built entirely on MSE API
- **Purpose**: All decoded media data flows through `SourceBuffer.appendBuffer()`
- **Impact**: Without MSE, playback is impossible
- **Reference**: [W3C MSE Specification](https://www.w3.org/TR/media-source/)

#### SharedArrayBuffer
- **Required**: For WebAssembly multi-threading (pthreads)
- **Security Headers**: Requires COOP/COEP headers (see [Installation](installation.md))
- **Impact**: Without SharedArrayBuffer, multi-threading fails and player may not initialize
- **Headers Required**:
  ```
  Cross-Origin-Opener-Policy: same-origin
  Cross-Origin-Embedder-Policy: require-corp
  ```

#### Other Requirements
- **WebAssembly**: Required for core functionality
- **HTTPS**: Required for SharedArrayBuffer and secure contexts
- **HTMLMediaElement**: Required (`<video>` or `<audio>` element)

### Known Limitations
- ⚠️ **Browser-Only**: Cannot be used outside browser environments
- ⚠️ **iOS Memory**: Maximum 1GB WASM memory limit
- ⚠️ **WebView Support**: Limited due to SharedArrayBuffer requirements (especially WebView)
- ⚠️ **Single Instance**: One player instance per page (MSE implementation limitation)
- ⚠️ **Server-Side**: Not suitable for Node.js or server-side media processing

### Platform Support Matrix

| Platform | MSE Support | SharedArrayBuffer | Status |
|----------|-------------|-------------------|--------|
| Web Browsers | ✅ | ✅ (with headers) | ✅ Fully Supported |
| Electron | ✅ | ✅ (with headers) | ✅ Supported |
| Tauri | ✅ | ✅ (with headers) | ✅ Supported |
| React Native (WebView) | ⚠️ Varies | ⚠️ Varies | ⚠️ Requires testing |
| Native Mobile Apps | ❌ | ❌ | ❌ Not supported (use WebView) |
| Node.js | ❌ | ❌ | ❌ Not supported |
| Server-Side | ❌ | ❌ | ❌ Not supported |

## Important Notes

### Browser-Only Architecture

This player's architecture is fundamentally tied to browser APIs:
- All decoded media flows through Media Source Extensions (MSE)
- Requires `SourceBuffer.appendBuffer()` for playback
- Cannot function without browser media pipeline
- See [MSE Dependency](../architecture/mse-dependency.md) for detailed explanation

### SharedArrayBuffer Requirement

Multi-threading support requires `SharedArrayBuffer`, which needs specific HTTP headers:
- See [Installation Guide](installation.md) for server configuration
- Without proper headers, player initialization will fail
- Critical for performance in multi-threaded scenarios

## Next Steps

- [Installation Guide](installation.md) - Set up the player and configure COOP/COEP headers
- [Quick Start](quick-start.md) - Get your first player instance running
- [Architecture Overview](../architecture/overview.md) - Understand the internals
- [MSE Dependency](../architecture/mse-dependency.md) - Learn about browser-only constraints
