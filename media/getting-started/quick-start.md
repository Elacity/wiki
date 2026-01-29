# Media Upload Quick Start

This guide shows you how to quickly get started with uploading and minting media content.

## Basic Setup

```typescript
import { ElacityClient } from '@elacity-js/api';
import { EthersAdapter, EthersAbiEncoder } from '@elacity-js/contracts-ethers-adapter';
import { MediaUploadService } from '@elacity-js/media';
import { ChainId } from '@elacity-js/core';
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

// 4. Create ABI encoder (Ethers implementation)
const abiEncoder = new EthersAbiEncoder();

// 5. Initialize media upload service
const mediaService = new MediaUploadService(
  apiClient,
  contractRunner,
  {
    abiEncoder,
    tokenInfo: {
      address: '0x0000000000000000000000000000000000000000', // ETH
      decimals: 18,
    },
  }
);
```

## Upload Media (Without Auto-Mint)

```typescript
const result = await mediaService.uploadMedia(
  {
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
  },
  {
    onProgress: (progress) => {
      console.log(`Upload progress: ${progress.progress}% - ${progress.step}`);
    },
    autoMint: false, // Don't mint automatically
  }
);

console.log('Upload complete! Request ID:', result.requestId);
// Later, you can mint using the requestId
```

## Upload and Auto-Mint

```typescript
const result = await mediaService.uploadMedia(
  {
    // ... same input as above
  },
  {
    onProgress: (progress) => {
      console.log(`Progress: ${progress.progress}%`);
    },
    autoMint: true, // Automatically mint after encoding completes
  }
);

console.log('Minted! Token ID:', result.tokenId);
console.log('Transaction hash:', result.txHash);
```

## Automatic Strategy Selection

The SDK uses the **Strategy pattern** to automatically select the best available listener strategy. No manual configuration needed!

**How It Works:**
- The `WorkflowListenerFactory` checks available strategies
- Selects the highest-priority available strategy:
  - **FirebaseListenerStrategy** (Priority 10): Real-time Firebase Firestore - preferred in browser
  - **PollingListenerStrategy** (Priority 1): Polls background job API - fallback option

**In Browser:**
- If Firebase Firestore is initialized → FirebaseListenerStrategy used
- If Firebase not available → PollingListenerStrategy used

**In Node.js:**
- PollingListenerStrategy used automatically (Firebase not available)

## Next Steps

- See [Media Upload Service](./media-upload-service.md) for detailed API reference
- See [Background Job Service](../api/services/background-job.md) for tracking workflows
- See [Channel Service](../api/services/channel.md) for channel management
