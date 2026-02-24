# Media Upload Service

> **Package**: `@elacity-js/media-packager`
> **Service**: `MediaUploadService`
> **Last Updated**: 2026-02-24

## Overview

The `MediaUploadService` orchestrates the complete media upload and minting workflow on the Elacity platform. It handles file uploads, transcoding, encoding, metadata generation, and optional blockchain minting.

Progress is tracked in real time through a `MediaUploadHandle` using a single workflow listener strategy selected at service construction time (default: WebSocket).

## Key Features

- **Complete Workflow Orchestration**: Handles all steps from upload to minting
- **Handle-Based Progress**: `execute()` returns an `IMediaUploadHandle` for real-time tracking
- **Workflow Listener Strategy**: WebSocket (default), long-polling, or custom strategy
- **Background Job Integration**: Automatic background job creation and tracking
- **Cross-Platform**: Works in both browser and Node.js environments
- **Flexible Minting**: Optional automatic minting or manual `mint()` call on the handle
- **Error Handling**: Comprehensive error handling with job status updates

## Installation

See [Installation Guide](../getting-started/installation.md) for setup instructions.

## Basic Usage

### Initialization

```typescript
import { ElacityClient } from '@elacity-js/api';
import { EthersAdapter, EthersAbiEncoder } from '@elacity-js/contracts-ethers-adapter';
import { MediaUploadService } from '@elacity-js/media-packager';
import { PollingProgressListenerStrategy } from '@elacity-js/media-packager/listeners';

const apiClient = new ElacityClient({ chainId: ChainId.BASE_MAINNET });
await apiClient.auth.login(address, signature);

const contractRunner = new EthersAdapter(signer);
const contractExecutor = new EthersTransactionExecutor(signer);
const abiEncoder = new EthersAbiEncoder();

const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  contractExecutor,
  {
    abiEncoder,
    baseUrl: 'https://api.ela.city/api', // optional override
    // listenerStrategy: new PollingProgressListenerStrategy({ pollInterval: 3000 }),
  }
);
```

### Upload Media

```typescript
// 1. Create a request
const request = await mediaService.createRequest({
  title: 'My Video',
  assetFile: videoFile,
  assetThumbnail: thumbnailFile,
  // ... other required fields
});

// 2. Execute the upload -- returns a handle, not a result
const handle = await mediaService.execute(request);

// 3. Track progress in real time
handle.onProgress((progress) => {
  console.log(`${progress.progress}% - ${progress.step} - ${progress.caption}`);
});

// 4. Start the configured listener strategy
handle.startListening();

// 5. Wait for encoding to finish
await handle.waitCompletionOf('generate_metadata');

// 6. Mint (if not using autoMint)
const mintResult = await mediaService.mint(handle);
console.log('Token ID:', mintResult.tokenId);
```

## API Reference

### Constructor

```typescript
new MediaUploadService(
  apiClient: ElacityClient,
  contractRunner: IContractRunner,
  contractExecutor: ITransactionExecutor,
  options?: {
    abiEncoder?: IAbiEncoder;
    baseUrl?: string;
    listenerStrategy?: WorkflowProgressListenerStrategy<MediaUploadInput>;
  }
)
```

**Parameters:**
- `apiClient`: Initialized ElacityClient instance
- `contractRunner`: Contract runner adapter (EthersAdapter or ViemAdapter)
- `contractExecutor`: Transaction executor for batching/sending on-chain calls
- `options`: Optional configuration
  - `abiEncoder`: ABI encoder implementation (`EthersAbiEncoder`, `ViemAbiEncoder`, or custom `IAbiEncoder`)
  - `baseUrl`: Override base URL for uploads
  - `listenerStrategy`: Custom strategy implementing `WorkflowProgressListenerStrategy`

### Methods

#### `createRequest(input): Promise<UploadRequest>`

Creates a new upload request, initializes the background job, and prepares the workflow state.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `input` | `MediaUploadInput` | The complete media configuration. |

**Returns:** `Promise<UploadRequest>` - A request object. Call `request.acquire()` to create the background job and obtain a handle.

---

#### `execute(request, options?): Promise<IMediaUploadHandle>`

Starts the upload workflow (thumbnail + media file) and returns a handle for tracking progress.

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `request` | `UploadRequest` | The request created via `createRequest`. |
| `options` | `MediaUploadOptions` | (Optional) `{ autoMint?: boolean }`. |

**Returns:** `Promise<IMediaUploadHandle>` -- a handle for subscribing to progress and awaiting steps.

```typescript
const handle = await mediaService.execute(request, { autoMint: false });

handle.onProgress((progress) => {
  console.log(`${progress.progress}% - ${progress.caption}`);
});

handle.startListening();
await handle.waitCompletionOf('generate_metadata');
```

---

#### `mint(handle): Promise<{ tokenId: string; txHash: string }>`

Mints the uploaded media as an NFT. Requires that the `generate_metadata` step has completed (`handle.mintable === true`).

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `handle` | `IMediaUploadHandle` | A handle whose encoding workflow has finished. |

**Returns:** `Promise<{ tokenId: string; txHash: string }>`

---

#### `resumeFromJob(requestId): Promise<IMediaUploadHandle>`

Restores a `MediaUploadHandle` from an existing background job. Use this to reconnect to an in-progress upload (e.g. after a page reload).

**Parameters:**
| Parameter | Type | Description |
| :--- | :--- | :--- |
| `requestId` | `string` | The background job request ID to resume. |

**Returns:** `Promise<IMediaUploadHandle>`

## Handle API (`IMediaUploadHandle`)

The handle returned by `execute()` exposes the following interface:

### Properties

| Property | Type | Description |
| :--- | :--- | :--- |
| `requestId` | `string` | Background job request ID. |
| `mintable` | `boolean` | `true` when `generate_metadata` step is completed. |
| `completion` | `number` | Overall workflow completion percentage (0-100). |

### Methods

#### `onProgress(callback): () => void`

Registers a callback to receive real-time progress updates from the active listener strategy. Returns an unsubscribe function.

```typescript
const unsubscribe = handle.onProgress((progress: UploadProgress) => {
  console.log(`${progress.progress}% - ${progress.step} - ${progress.caption}`);
});

// Later, to stop receiving updates:
unsubscribe();
```

#### `reportProgress(payload): void`

Manually injects a progress event (used internally by `MediaUploadService` during the upload phase).

#### `startListening(): void`

Starts the selected listener strategy. Safe to call multiple times.

#### `stopListening(): void`

Stops the selected listener strategy. The handle can be re-activated later by calling `startListening()` again.

#### `waitCompletionOf(step, options?): Promise<StepProgress>`

Waits for a specific workflow step to reach `COMPLETED` status. Automatically calls `startListening()` if not already active. Rejects if the step reaches `FAILED` or if the timeout expires.

```typescript
const stepResult = await handle.waitCompletionOf('generate_metadata');
// stepResult: { step: 'generate_metadata', completion: 100, status: 'COMPLETED' }

// With custom options:
await handle.waitCompletionOf('generate_metadata', {
  checkInterval: 1000,  // poll internal state every 1s (default: 500ms)
  timeoutMs: 300_000,   // timeout after 5 minutes (default: 10 minutes)
});
```

## Input Types

### `EnrichedPayload`

Payload enriched during the upload workflow with backend-generated data:

```typescript
interface EnrichedPayload {
  thumbnailHash?: string;  // IPFS hash, set after thumbnail upload
}
```

### `MediaUploadInput`

Complete media upload configuration. Extends `EnrichedPayload`:

```typescript
interface MediaUploadInput extends EnrichedPayload {
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

  // Publisher
  publisher: string;                // Publisher wallet address

  // Royalties
  royalties: RoyaltyShare[];
}
```

### `MediaUploadOptions`

Upload configuration options:

```typescript
interface MediaUploadOptions {
  autoMint?: boolean;  // Default: false
}
```

> **Note:** The `onProgress` callback has moved to the handle. Use `handle.onProgress()` instead of passing it in options.

### `UploadProgress`

Progress event payload:

```typescript
interface UploadProgress {
  progress: number;              // 0-100
  step?: string;                 // Step identifier
  caption?: string;              // Human-readable description
  data?: Record<string, unknown>; // Additional data
}
```

### `StepProgress`

Per-step progress information:

```typescript
interface StepProgress {
  step: string;
  completion: number;  // 0-100
  status: string;      // e.g. 'PENDING', 'PROCESSING', 'COMPLETED', 'FAILED'
}
```

### `WorkflowProgressPayload`

WebSocket message payload from the `wfp-socket` service:

```typescript
interface WorkflowProgressPayload {
  type: 'update';
  requestId: string;
  timestamp: string;  // ISO 8601
  data: {
    completion: number;
    currentStep: {
      lastUpdate: string;
      step: string;
      caption: string;
      progress: number;
      status: string;
    };
  };
}
```

### `IEvent<Ev, T>`

Minimal typed event emitter used internally by `MediaUploadHandle`:

```typescript
interface IEvent<Ev, T> {
  on(name: Ev, callback: (data: T) => void): () => void;
  emit(name: Ev, data: T): void;
  off(name: Ev, callback?: (data: T) => void): void;
}
```

### `IMediaUploadHandle`

Interface implemented by `MediaUploadHandle`:

```typescript
interface IMediaUploadHandle {
  requestId: string;
  mintable: boolean;
  completion: number;
  startListening(): void;
  stopListening(): void;
  waitCompletionOf(
    step: string,
    options?: { checkInterval?: number; timeoutMs?: number }
  ): Promise<StepProgress>;
  reportProgress(payload: UploadProgress): void;
  onProgress(callback: UploadProgressCallback): () => void;
}
```

## Workflow Steps

The service handles these steps automatically:

1. **Create Background Job** (0%)
   - Creates a background job for tracking progress

2. **Upload Thumbnail** (Contribution: 5% weight)
   - Uploads thumbnail to IPFS

3. **Upload Media File** (Contribution: 40% weight)
   - Uploads media file to backend
   - Progress is tracked via session and mapped to the overall completion

4. **Transcode & Encode** (Contribution: 10% + 40% weights)
   - Backend automatically processes transcode and encoding
   - Progress tracked through the selected workflow listener strategy
   - WebSocket mode uses `wfp-socket`; polling mode queries background jobs periodically
   - Encoding result is extracted from the job payload

5. **Generate Metadata** (Contribution: 5% weight)
   - Generates IPFS metadata URI

6. **Mint to Blockchain** (Contribution: 5% weight, optional)
   - Formats mint data and executes transaction via `contractExecutor`

## Progress Tracking

Progress is tracked through `MediaUploadHandle` using a single selected strategy:

```typescript
const handle = await mediaService.execute(request);

// Subscribe to progress events
handle.onProgress((progress: UploadProgress) => {
  console.log(`Progress: ${progress.progress}%`);
  console.log(`Step: ${progress.step}`);
  console.log(`Caption: ${progress.caption}`);
});

// Start selected strategy
handle.startListening();

// Wait for a specific step
await handle.waitCompletionOf('generate_metadata');
```

**Progress Ranges:**
- 0-5%: Creating background job + uploading thumbnail
- 5-40%: Uploading media file
- 40-90%: Transcoding + encoding (tracked by configured strategy)
- 90-95%: Generating metadata
- 95-100%: Minting to blockchain (if autoMint enabled)

## Error Handling

The service automatically updates background job status on errors:

```typescript
try {
  const handle = await mediaService.execute(request);
  handle.startListening();
  await handle.waitCompletionOf('generate_metadata');
} catch (error) {
  // Job status is automatically set to FAILED
  const job = await apiClient.backgroundJobs.retrieveBackgroundJob(handle.requestId);
  console.error('Upload failed:', job.steps?.find(s => s.status === 'FAILED'));
}
```

## Frontend vs Backend Usage

### Frontend (Browser)

```typescript
const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  contractExecutor,
  { abiEncoder }
);

const request = await mediaService.createRequest(input);
const handle = await mediaService.execute(request);

handle.onProgress((p) => updateUI(p));
handle.startListening(); // WebSocket strategy by default
```

**Automatic Behavior:**
- Default strategy is WebSocket
- WebSocket connects to `wfp-socket/ws/{requestId}` for push-based progress
- XMLHttpRequest progress tracking for file uploads

### Backend (Node.js)

```typescript
const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  contractExecutor,
  {
    abiEncoder,
    listenerStrategy: new PollingProgressListenerStrategy({ pollInterval: 3000 }),
  }
);

const request = await mediaService.createRequest(input);
const handle = await mediaService.execute(request);

handle.onProgress((p) => console.log(p));
handle.startListening(); // Polling strategy

await handle.waitCompletionOf('generate_metadata');
```

**Automatic Behavior:**
- Poll interval is configured by the polling strategy instance
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

## Complete Example

```typescript
import { ElacityClient } from '@elacity-js/api';
import { EthersAdapter, EthersTransactionExecutor } from '@elacity-js/contracts-ethers-adapter';
import { MediaUploadService } from '@elacity-js/media-packager';
import { ChainId } from '@elacity-js/common';
import { defaultAbiCoder } from '@ethersproject/abi';
import { ethers } from 'ethers';

async function uploadVideo() {
  // 1. Setup
  const apiClient = new ElacityClient({ chainId: ChainId.BASE_MAINNET });
  await apiClient.auth.login(address, signature);

  const provider = new ethers.BrowserProvider(window.ethereum);
  const signer = await provider.getSigner();
  const contractRunner = new EthersAdapter(signer);
  const contractExecutor = new EthersTransactionExecutor(signer);

  const mediaService = new MediaUploadService(
    apiClient,
    contractRunner,
    contractExecutor,
    {
      abiEncoder: (types, values) => defaultAbiCoder.encode(types, values),
    }
  );

  // 2. Create request
  const request = await mediaService.createRequest({
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
    publisher: creatorAddress,
    categories: ['Music'],
    previewEnabled: true,
    previewDuration: 30,
    royalties: [
      { identifier: 'A', address: creatorAddress, royalty: 95 },
    ],
  });

  // 3. Execute upload -- returns a handle
  const handle = await mediaService.execute(request);

  // 4. Track progress
  handle.onProgress((progress) => {
    console.log(`${progress.progress}% - ${progress.step} - ${progress.caption}`);
  });

  // 5. Start listener strategy
  handle.startListening();

  // 6. Wait for encoding to complete
  await handle.waitCompletionOf('generate_metadata');

  // 7. Mint
  const mintResult = await mediaService.mint(handle);
  console.log('Minted! Token ID:', mintResult.tokenId);

  // 8. Clean up
  handle.stopListening();

  return mintResult;
}
```

## Related Documentation

- [Background Job Service](../../api/services/background-job.md) - Workflow tracking
- [Channel Service](../../api/services/channel.md) - Channel management
- [Contracts SDK](../../contracts/sdk/channel.md) - Smart contract interactions
