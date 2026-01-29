# Media Upload Service

> **Package**: `@elacity-js/media`  
> **Service**: `MediaUploadService`

## Overview

The `MediaUploadService` orchestrates the complete media upload and minting workflow on the Elacity platform. It handles file uploads, transcoding, encoding, metadata generation, and optional blockchain minting.

## Key Features

- **Complete Workflow Orchestration**: Handles all steps from upload to minting
- **Progress Tracking**: Real-time progress updates via callbacks
- **Background Job Integration**: Automatic background job creation and tracking
- **Cross-Platform**: Works in both browser and Node.js environments
- **Flexible Minting**: Optional automatic minting or manual control
- **Error Handling**: Comprehensive error handling with job status updates

## Installation

See [Installation Guide](../getting-started/installation.md) for setup instructions.

## Basic Usage

### Initialization

```typescript
import { ElacityClient } from '@elacity-js/api';
import { EthersAdapter } from '@elacity-js/contracts-ethers-adapter';
import { MediaUploadService } from '@elacity-js/media';
import { defaultAbiCoder } from '@ethersproject/abi';

const apiClient = new ElacityClient({ chainId: ChainId.BASE_MAINNET });
await apiClient.auth.login(address, signature);

const contractRunner = new EthersAdapter(signer);

const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  {
    abiEncoder: (types, values) => defaultAbiCoder.encode(types, values),
    tokenInfo: { address: '0x...', decimals: 18 },
  }
);
```

### Upload Media

```typescript
const result = await mediaService.uploadMedia(
  {
    title: 'My Video',
    assetFile: videoFile,
    assetThumbnail: thumbnailFile,
    // ... other required fields
  },
  {
    onProgress: (progress) => console.log(`${progress.progress}%`),
    autoMint: false,
  }
);
```

## API Reference

### Constructor

```typescript
new MediaUploadService(
  apiClient: ElacityClient,
  contractRunner: IContractRunner,
  options?: {
    workflowListener?: WorkflowListener;
    abiEncoder?: ABIEncoder;
    tokenInfo?: TokenInfo;
    baseUrl?: string;
  }
)
```

**Parameters:**
- `apiClient`: Initialized ElacityClient instance
- `contractRunner`: Contract runner adapter (EthersAdapter or ViemAdapter)
- `options`: Optional configuration
  - `workflowListener`: Legacy custom workflow listener (deprecated - use Strategy pattern instead)
  - `abiEncoder`: ABI encoding function (required for minting)
  - `tokenInfo`: Token information for price encoding (defaults to 18 decimals)
  - `baseUrl`: Override base URL for uploads

**Listener Strategy (Automatic):**

The SDK uses the **Strategy pattern** to automatically select the best listener:

1. **FirebaseListenerStrategy** (Priority 10): Auto-detects Firebase Firestore if initialized
   - Real-time updates via Firestore listeners
   - Preferred for browser environments
   - Automatically used if Firebase is available

2. **PollingListenerStrategy** (Priority 1): Falls back to polling
   - Polls background job API every 2 seconds
   - Always available when `BackgroundJobService` is provided
   - Used automatically if Firebase is not available

The `WorkflowListenerFactory` handles strategy selection automatically. No configuration needed!

### Methods

#### `uploadMedia(input, options): Promise<MediaUploadResult>`

Uploads media content and optionally mints it as an NFT.

**Parameters:**
- `input`: `MediaUploadInput` - Media upload configuration
- `options`: `MediaUploadOptions` - Upload options

**Returns:** `Promise<MediaUploadResult>`

**Example:**
```typescript
const result = await mediaService.uploadMedia(
  {
    title: 'My Video',
    description: 'Video description',
    assetFile: videoFile,
    assetThumbnail: thumbnailFile,
    pricePerSale: 4.99,
    copiesNumber: 10000,
    accessMethod: 'buy_once',
    priceCurrency: '0x0000000000000000000000000000000000000000',
    channel: '0x...',
    gateway: '0x...',
    categories: ['Music'],
    previewEnabled: true,
    previewDuration: 30,
    royalties: [
      { identifier: 'A', address: creatorAddress, royalty: 95 },
    ],
  },
  {
    onProgress: (progress) => {
      console.log(`${progress.progress}% - ${progress.step}`);
    },
    autoMint: true,
  }
);
```

## Input Types

### `MediaUploadInput`

Complete media upload configuration:

```typescript
interface MediaUploadInput {
  // Content Metadata
  title: string;                    // 3-100 characters
  description?: string;              // Optional, max 2000 chars
  assetFile: File | FileLike;        // Video/audio file
  assetThumbnail: File | FileLike;  // Thumbnail image

  // Pricing & Access
  pricePerSale: number;             // 0.001 - 1,000,000
  copiesNumber: number;             // 1 - 10,000
  accessMethod: 'free' | 'buy_once' | 'buy_and_resell';
  priceCurrency: string;            // Token contract address
  resellerCut?: number;             // 10-90% (for resellable)

  // Distribution
  channel: string;                  // Channel contract address
  gateway: string;                  // Authority gateway address
  categories: string[];             // 1-5 categories required
  tags?: string[];                  // Optional tags

  // Preview Settings
  previewEnabled: boolean;
  previewDuration: number;          // Seconds

  // Royalties
  royalties: Array<{
    identifier: string;             // 'A' = Creator, 'E' = Platform
    address: string;
    royalty: number;                // Percentage (0-100)
  }>;
}
```

### `MediaUploadOptions`

Upload configuration options:

```typescript
interface MediaUploadOptions {
  onProgress?: (progress: UploadProgress) => void;
  autoMint?: boolean;  // Default: false
}
```

### `MediaUploadResult`

Result returned after upload:

```typescript
interface MediaUploadResult {
  requestId: string;           // Background job ID
  channelAddress: string;        // Channel contract address
  tokenId?: string;             // Token ID (if autoMint was true)
  txHash?: string;              // Transaction hash (if autoMint was true)
}
```

## Workflow Steps

The service handles these steps automatically:

1. **Create Background Job** (0%)
   - Creates a background job for tracking progress

2. **Upload Thumbnail** (5%)
   - Uploads thumbnail to IPFS
   - Returns IPFS CID

3. **Upload Media File** (10-40%)
   - Uploads media file to backend
   - Triggers transcode workflow
   - Returns uploaded path and requestId

4. **Wait for Transcode** (40-50%)
   - Polls background job for transcode completion
   - Updates progress as transcode progresses

5. **Request Encoding** (50-90%)
   - Requests DASH encoding with DRM protection
   - Waits for encoding completion
   - Returns encode result with CID, KID, and metadata

6. **Generate Metadata** (90-95%)
   - Generates IPFS metadata URI
   - Includes thumbnail, encode result, and form data

7. **Mint to Blockchain** (95-100%, optional)
   - Formats mint data
   - Executes mint transaction
   - Extracts token ID from transaction

## Progress Tracking

The `onProgress` callback receives updates throughout the workflow:

```typescript
onProgress: (progress: UploadProgress) => {
  console.log(`Progress: ${progress.progress}%`);
  console.log(`Step: ${progress.step}`);
  console.log(`Data:`, progress.data);
}
```

**Progress Ranges:**
- 0-5%: Creating background job
- 5-10%: Uploading thumbnail
- 10-40%: Uploading media file
- 40-50%: Transcoding media
- 50-90%: Encoding media (DASH + DRM)
- 90-95%: Generating metadata
- 95-100%: Minting to blockchain (if autoMint enabled)

## Error Handling

The service automatically updates background job status on errors:

```typescript
try {
  const result = await mediaService.uploadMedia(input, options);
} catch (error) {
  // Job status is automatically set to FAILED
  // You can retrieve the job to see error details
  const job = await apiClient.backgroundJobs.retrieveBackgroundJob(requestId);
  console.error('Upload failed:', job.steps?.find(s => s.status === 'FAILED'));
}
```

## Frontend vs Backend Usage

### Frontend (Browser)

```typescript
// Strategy pattern automatically selects Firebase if available
const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  {
    abiEncoder,
    // No workflowListener needed - Strategy pattern handles it
  }
);
```

**Automatic Strategy Selection:**
- **FirebaseListenerStrategy** is selected if Firebase Firestore is initialized
- Provides real-time encoding completion updates via Firestore listeners
- Falls back to **PollingListenerStrategy** if Firebase unavailable
- XMLHttpRequest progress tracking for file uploads

**Custom Strategy (Advanced):**
You can provide a custom listener that implements the `WorkflowListener` interface:

```typescript
import { WorkflowListenerStrategy } from '@elacity-js/media';

class CustomListenerStrategy implements WorkflowListenerStrategy {
  isAvailable() { return true; }
  getPriority() { return 7; }
  async listenForCompletion(requestId: string): Promise<EncodeResult> {
    // Your custom implementation
  }
}

const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  {
    abiEncoder,
    workflowListener: {
      // Legacy interface - wrapped automatically
      listenEncodeResult: (requestId, callbacks) => { /* ... */ },
    },
  }
);
```

### Backend (Node.js)

```typescript
// PollingListenerStrategy is automatically selected
const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  {
    abiEncoder,
  }
);
```

**Automatic Behavior:**
- **PollingListenerStrategy** is selected (Firebase not available in Node.js)
- Polls background job API every 2 seconds
- Maximum 10-minute timeout
- Suitable for server-side processing

**Requirements:**
- Install `form-data` package for FormData support
- Background job service must be available via `apiClient.backgroundJobs`

## ABI Encoder Setup

The service requires an ABI encoder function for minting. Examples:

### Ethers.js

```typescript
import { defaultAbiCoder } from '@ethersproject/abi';

const abiEncoder = (types: string[], values: unknown[]) =>
  defaultAbiCoder.encode(types, values);
```

### Viem

```typescript
import { encodeAbiParameters } from 'viem';

const abiEncoder = (types: string[], values: unknown[]) => {
  const params = types.map((type, i) => ({
    type: type as any,
    name: `param${i}`,
  }));
  return encodeAbiParameters(params, values);
};
```

## Token Information

For price encoding, provide token information:

```typescript
const tokenInfo = {
  address: '0x0000000000000000000000000000000000000000', // ETH
  decimals: 18,
};

// Or for USDC
const tokenInfo = {
  address: '0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913', // Base USDC
  decimals: 6,
};
```

## Complete Example

```typescript
import { ElacityClient } from '@elacity-js/api';
import { EthersAdapter } from '@elacity-js/contracts-ethers-adapter';
import { MediaUploadService } from '@elacity-js/media';
import { ChainId } from '@elacity-js/core';
import { defaultAbiCoder } from '@ethersproject/abi';
import { ethers } from 'ethers';

async function uploadVideo() {
  // 1. Setup
  const apiClient = new ElacityClient({ chainId: ChainId.BASE_MAINNET });
  await apiClient.auth.login(address, signature);

  const provider = new ethers.BrowserProvider(window.ethereum);
  const signer = await provider.getSigner();
  const contractRunner = new EthersAdapter(signer);

  const mediaService = new MediaUploadService(apiClient, contractRunner, {
    abiEncoder: (types, values) => defaultAbiCoder.encode(types, values),
    tokenInfo: { address: '0x0', decimals: 18 },
  });

  // 2. Upload
  const result = await mediaService.uploadMedia(
    {
      title: 'My Awesome Video',
      description: 'A great video',
      assetFile: videoFile,
      assetThumbnail: thumbnailFile,
      pricePerSale: 4.99,
      copiesNumber: 10000,
      accessMethod: 'buy_once',
      priceCurrency: '0x0000000000000000000000000000000000000000',
      channel: '0x...',
      gateway: '0x...',
      categories: ['Music'],
      previewEnabled: true,
      previewDuration: 30,
      royalties: [
        { identifier: 'A', address: creatorAddress, royalty: 95 },
      ],
    },
    {
      onProgress: (progress) => {
        console.log(`${progress.progress}% - ${progress.step}`);
      },
      autoMint: true,
    }
  );

  console.log('Uploaded! Token ID:', result.tokenId);
  return result;
}
```

## Related Documentation

- [Background Job Service](../api/services/background-job.md) - Workflow tracking
- [Channel Service](../api/services/channel.md) - Channel management
- [Contracts SDK](../contracts/sdk/channel.md) - Smart contract interactions
