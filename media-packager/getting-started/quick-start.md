# Media Upload Quick Start

> **Last Updated**: 2026-02-23

This guide shows you how to quickly get started with uploading and minting media content.

## Basic Setup

```typescript
import { ElacityClient } from '@elacity-js/api';
import { EthersAdapter, EthersAbiEncoder, EthersTransactionExecutor } from '@elacity-js/contracts-ethers-adapter';
import { MediaUploadService } from '@elacity-js/media-packager';
import { PollingProgressListenerStrategy } from '@elacity-js/media-packager/listeners';
import { ChainId } from '@elacity-js/common';
import { ethers } from 'ethers';

// 1. Initialize API client
const apiClient = new ElacityClient({
  chainId: ChainId.BASE_MAINNET,
});

// 2. Authenticate
await apiClient.auth.login(address, signature);

// 3. Setup contract runner (ethers.js example)
const provider = new ethers.BrowserProvider(window.ethereum);
const signer = await provider.getSigner();
const contractRunner = new EthersAdapter(signer);
const contractExecutor = new EthersTransactionExecutor(signer);

// 4. Create ABI encoder (Ethers implementation)
const abiEncoder = new EthersAbiEncoder();

// 5. Initialize media upload service
const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  contractExecutor,
  {
    abiEncoder,
  }
);
```

### Optional: Choose Workflow Listener Strategy

By default, the workflow uses the `websocket` listener strategy. You can force long polling or provide a custom strategy:

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
```

## Upload Media (Without Auto-Mint)

```typescript
// 1. Create a request
const request = await mediaService.createRequest({
  title: 'My Awesome Video',
  description: 'A great video about...',
  assetFile: videoFile, // File object
  assetThumbnail: thumbnailFile, // File object
  pricePerSale: 4.99,
  copiesNumber: 10000,
  accessMethod: 'buy_once',
  priceCurrency: '0x0000000000000000000000000000000000000000', // ETH
  channel: '0x...', // Channel contract address
  gateway: '0x...', // Gateway contract address
  publisher: creatorAddress,
  categories: ['Music', 'Entertainment'],
  tags: ['music', 'video'],
  previewEnabled: true,
  previewDuration: 30,
  royalties: [
    {
      identifier: 'A',
      address: creatorAddress,
      royalty: 95,
    },
  ],
});

// 2. Execute the upload -- returns a handle
const handle = await mediaService.execute(request);

// 3. Subscribe to progress updates
handle.onProgress((progress) => {
  console.log(`Upload progress: ${progress.progress}% - ${progress.step} - ${progress.caption}`);
});

// 4. Start the configured listener strategy
handle.startListening();

// 5. Wait for encoding to complete
await handle.waitCompletionOf('generate_metadata');

console.log('Upload complete! Request ID:', handle.requestId);
// Later, you can mint using: await mediaService.mint(handle);
```

## Upload and Mint

```typescript
// 1. Create request and execute
const request = await mediaService.createRequest({
  // ... same input as above
});
const handle = await mediaService.execute(request);

// 2. Track progress
handle.onProgress((progress) => {
  console.log(`Progress: ${progress.progress}% - ${progress.caption}`);
});

// 3. Start listening and wait for encoding
handle.startListening();
await handle.waitCompletionOf('generate_metadata');

// 4. Mint
const mintResult = await mediaService.mint(handle);
console.log('Minted! Token ID:', mintResult.tokenId);
console.log('Transaction hash:', mintResult.txHash);

// 5. Clean up
handle.stopListening();
```

## Resume an In-Progress Upload

If the user refreshes the page or reconnects, you can resume tracking an existing upload:

```typescript
const handle = await mediaService.resumeFromJob(savedRequestId);

handle.onProgress((progress) => {
  console.log(`Resumed: ${progress.progress}% - ${progress.caption}`);
});

handle.startListening();
await handle.waitCompletionOf('generate_metadata');
```

## Progress Tracking

`MediaUploadHandle` runs one workflow listener strategy for the full handle lifecycle:

- **WebSocket** (default): push-based updates from `wfp-socket/ws/{requestId}`.
- **Long polling** (`new PollingProgressListenerStrategy({ pollInterval })`): periodic background-job sync via API.
- **Custom** (`listenerStrategy`): your own `WorkflowProgressListenerStrategy` implementation.

All strategies emit the same normalized `UploadProgress` payload through `handle.onProgress()`.

## Next Steps

- See [Media Upload Service](../services/media-upload-service.md) for detailed API reference
- See [Workflow Architecture](../architecture/workflow.md) for the complete flow diagram
- See [Listener Strategies](../architecture/listener-strategies.md) for strategy interface details
- See [Background Job Service](../../api/services/background-job.md) for tracking workflows
- See [Channel Service](../../api/services/channel.md) for channel management
