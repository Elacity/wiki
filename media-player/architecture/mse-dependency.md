# Media Source Extensions (MSE) Dependency

**⚠️ CRITICAL ARCHITECTURAL CONSTRAINT**: The Elacity Media Player is built entirely on top of the [Media Source Extensions (MSE)](https://www.w3.org/TR/media-source/) API, which means it **only works in web browsers** and cannot be used in non-browser environments.

## What is Media Source Extensions?

Media Source Extensions (MSE) is a W3C specification that extends `HTMLMediaElement` to allow JavaScript to generate media streams for playback. It enables JavaScript applications to:

- Construct media streams dynamically
- Append media segments to a `SourceBuffer`
- Control adaptive streaming and buffering
- Handle time-shifting and live streams

The MSE API provides the `MediaSource` and `SourceBuffer` interfaces that allow JavaScript to feed decoded media data directly to the browser's media pipeline.

## Why MSE is Critical

The Elacity Media Player's architecture fundamentally depends on MSE:

```
┌─────────────────────────────────────────────────────────┐
│  C Core (WASM) - Media Processing                       │
│  - DASH parsing                                         │
│  - Segment decryption                                   │
│  - Media decoding (via FFmpeg)                          │
│  - Remuxing to browser-compatible format                │
└────────────────────┬────────────────────────────────────┘
                      │
                      │ Decoded media data
                      │ (video/audio frames)
                      ▼
┌─────────────────────────────────────────────────────────┐
│  JavaScript Layer                                       │
│  - Receives decoded frames from WASM                    │
│  - Appends to SourceBuffer via MSE API                  │
└────────────────────┬────────────────────────────────────┘
                      │
                      │ MSE API (Browser-only)
                      ▼
┌─────────────────────────────────────────────────────────┐
│  Browser Media Pipeline                                 │
│  - SourceBuffer.appendBuffer()                          │
│  - Native media rendering                               │
│  - HTMLMediaElement playback                            │
└─────────────────────────────────────────────────────────┘
```

**Key Point**: All decoded media data from the WASM remuxer is sent directly to the browser's `SourceBuffer` via `appendBuffer()`. There is no alternative rendering path - the player **cannot function without MSE**.

## Browser-Only Limitation

### What This Means

- ✅ **Works**: Web browsers (Chrome, Firefox, Safari, Edge)
- ✅ **Works**: Browser-based applications (Electron, Tauri with browser engine)
- ❌ **Does NOT Work**: Node.js, Deno, or any non-browser JavaScript runtime
- ❌ **Does NOT Work**: Native mobile apps (React Native, Flutter, etc.) without WebView
- ❌ **Does NOT Work**: Desktop applications without browser engine
- ❌ **Does NOT Work**: Server-side rendering or media processing

### Why No Alternative?

The player's design assumes:
1. **MSE API availability**: `MediaSource`, `SourceBuffer`, `SourceBufferList`
2. **HTMLMediaElement integration**: Direct attachment to `<video>` or `<audio>` elements
3. **Browser media pipeline**: Native decoding and rendering handled by the browser

There is no fallback mechanism or alternative rendering path. The decoded media frames **must** flow through MSE's `SourceBuffer.appendBuffer()` method.

## SharedArrayBuffer Requirement

The player uses WebAssembly multi-threading (pthreads) for parallel media processing, which requires `SharedArrayBuffer` support.

### Why SharedArrayBuffer?

- **Multi-threading**: WASM pthreads need shared memory for thread communication
- **Performance**: Parallel segment processing improves playback performance
- **Memory efficiency**: Shared memory reduces data copying overhead

### Browser Requirements

`SharedArrayBuffer` requires specific security headers due to [Spectre/Meltdown mitigations](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer#security_requirements):

**Required HTTP Headers:**
```
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp
```

**Without these headers:**
- `SharedArrayBuffer` is `undefined`
- Multi-threading fails
- Player initialization fails
- Playback cannot start

### Browser Support Matrix

| Browser | MSE Support | SharedArrayBuffer | COOP/COEP Required | Status |
|---------|-------------|-------------------|-------------------|--------|
| Chrome 90+ | ✅ | ✅ | ✅ | ✅ Fully Supported |
| Firefox 89+ | ✅ | ✅ | ✅ | ✅ Fully Supported |
| Safari 15.2+ | ✅ | ✅ | ✅ | ✅ Supported (with limitations) |
| Edge 90+ | ✅ | ✅ | ✅ | ✅ Fully Supported |
| iOS Safari | ✅ | ⚠️ Limited | ✅ | ⚠️ Limited (1GB memory limit) |
| WebView (iOS) | ✅ | ❌ | N/A | ❌ Not Supported |
| WebView (Android) | ✅ | ⚠️ Varies | ✅ | ⚠️ Varies by version |

### Checking Support

```javascript
// Check MSE support
if (!window.MediaSource || !MediaSource.isTypeSupported) {
  console.error('Media Source Extensions not supported');
}

// Check SharedArrayBuffer support
if (typeof SharedArrayBuffer === 'undefined') {
  console.error('SharedArrayBuffer not available. Check COOP/COEP headers.');
}

// Check specific codec support
const mimeType = 'video/mp4; codecs="avc1.42E01E"';
if (!MediaSource.isTypeSupported(mimeType)) {
  console.error(`Codec not supported: ${mimeType}`);
}
```

## Architecture Implications

### Data Flow Constraint

The entire playback pipeline is designed around MSE:

1. **WASM decodes media** → Produces decoded frames
2. **JavaScript receives frames** → Formats for MSE
3. **MSE SourceBuffer.appendBuffer()** → Only way to feed browser
4. **Browser renders** → Native media pipeline

**There is no alternative path.** The player cannot:
- Render to canvas directly
- Use WebGL for video rendering
- Output to file or stream
- Work without a browser media element

### Single Instance Limitation

Due to MSE implementation details, the current player architecture supports:
- ⚠️ **One player instance per page** (MSE limitation)
- Multiple instances may conflict with global state

This is a known limitation documented in the [STATUS.md](https://github.com/elacity/media-player/blob/main/STATUS.md).

## Integration Considerations

### For Web Applications

✅ **Perfect Fit:**
- Single-page applications (React, Vue, Angular)
- Progressive Web Apps (PWA)
- Browser extensions
- Electron/Tauri desktop apps

### For Mobile Applications

⚠️ **Requires WebView:**
- React Native: Use `WebView` component
- Flutter: Use `webview_flutter` plugin
- Native iOS/Android: Embed `WKWebView` or `WebView`

**Important**: WebView implementations vary in MSE and SharedArrayBuffer support. Test thoroughly.

### For Server-Side Applications

❌ **Not Suitable:**
- Node.js media processing
- Server-side rendering
- Media transcoding pipelines
- Backend media analysis

**Alternative**: Use FFmpeg directly or other server-side media processing tools.

## Migration Considerations

If you need to support non-browser environments:

1. **Separate implementations**: Use the player for browser, FFmpeg/other tools for server
2. **Hybrid approach**: Browser player for playback, server tools for processing
3. **WebView embedding**: Wrap browser player in WebView for mobile apps

## Related Documentation

- [W3C MSE Specification](https://www.w3.org/TR/media-source/) - Official MSE standard
- [Browser Compatibility](media-formats.md#browser-compatibility-matrix) - Codec support matrix
- [Installation](../getting-started/installation.md) - Server configuration for COOP/COEP headers
- [Troubleshooting](../development/troubleshooting.md) - Common MSE-related issues
