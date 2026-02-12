# Media Package Documentation

## Overview

The `@elacity-js/media-packager` package provides tools for uploading, processing, and minting media content (videos, audio) as NFTs on the Elacity platform.

## Getting Started

- [Installation](getting-started/installation.md) - Setup and dependencies
- [Quick Start](getting-started/quick-start.md) - Get up and running quickly

## Architecture

- [Workflow Architecture](architecture/workflow.md) - Complete workflow breakdown and component architecture

## Services

- [Media Upload Service](services/media-upload-service.md) - Complete upload and minting workflow

## Troubleshooting

- [Common Issues](troubleshooting.md) - Solutions to common problems

## Dependencies

The media package uses services from other packages:

- **`@elacity-js/api`** - API client and services
  - [Background Job Service](../api/services/background-job.md) - Track long-running media processing workflows
- **`@elacity-js/contracts`** - Smart contract interactions for minting
- **`@elacity-js/common`** - Common utilities and types

## Architecture

- **Background Jobs**: Track upload, transcoding, encoding, and minting progress
- **Step-Based Workflows**: Granular progress tracking for each workflow stage
- **WebSocket + Polling**: Dual-strategy real-time progress via `wfp-socket` WebSocket with automatic polling fallback
- **Handle-Based API**: `MediaUploadHandle` encapsulates progress tracking, event subscription, and step completion
- **Cross-Platform**: Works in both browser and Node.js environments
- **Integration**: Seamlessly integrates with API SDK and Contracts SDK

### Progress Strategy

The SDK uses a **dual-strategy** approach to track workflow progress in real time:

- **WebSocket** (`wfp-socket`): Push-based updates from the backend via `WorkflowProgressPayload` messages. Preferred when available.
- **Polling** (fallback): Periodically fetches the background job state from the API. Always available in both browser and Node.js.

The `MediaUploadHandle` manages both strategies internally. No manual configuration needed -- call `handle.startListening()` and both channels activate automatically.

## Workflow Overview

The media upload process follows these steps:

1. **Create Request** - Build an `UploadRequest` with `createRequest()`
2. **Execute Upload** - Call `execute()` to start; returns a `MediaUploadHandle`
3. **Upload Thumbnail** - Upload to IPFS
4. **Upload Media File** - Upload to backend (triggers transcode)
5. **Track Progress** - WebSocket + polling via `handle.onProgress()` / `handle.waitCompletionOf()`
6. **Generate Metadata** - Create IPFS metadata (backend)
7. **Mint to Blockchain** - Call `handle.mint()` or use `autoMint` option

## Related Packages

- `@elacity-js/api` - API client and services
- `@elacity-js/contracts` - Smart contract interactions
- `@elacity-js/common` - Common utilities and types
