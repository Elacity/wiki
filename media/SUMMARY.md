# Media Package Documentation

## Overview

The `@elacity-js/media` package provides tools for uploading, processing, and minting media content (videos, audio) as NFTs on the Elacity platform.

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
- **Strategy Pattern**: Automatic listener strategy selection (Firebase real-time or polling fallback)
- **Cross-Platform**: Works in both browser and Node.js environments
- **Integration**: Seamlessly integrates with API SDK and Contracts SDK

### Listener Strategy

The SDK uses the **Strategy pattern** to automatically select the best available listener for encoding completion:

- **FirebaseListenerStrategy** (Priority: 10): Real-time Firebase Firestore listener (preferred, auto-detected)
- **PollingListenerStrategy** (Priority: 1): Polls background job API (fallback, always available)

The `WorkflowListenerFactory` automatically selects the best strategy based on availability. No manual configuration needed!

## Workflow Overview

The media upload process follows these steps:

1. **Create Background Job** - Initialize tracking
2. **Upload Thumbnail** - Upload to IPFS
3. **Upload Media File** - Upload to backend (triggers transcode)
4. **Wait for Transcode** - Poll for completion
5. **Request Encoding** - DASH + DRM encoding
6. **Generate Metadata** - Create IPFS metadata
7. **Mint to Blockchain** - Optional NFT minting

## Related Packages

- `@elacity-js/api` - API client and services
- `@elacity-js/contracts` - Smart contract interactions
- `@elacity-js/common` - Common utilities and types
