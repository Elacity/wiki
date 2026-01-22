# API Package Overview

The `@elacity-js/api` package is the primary entry point for interacting with Elacity's backend services. It provides a type-safe, modular client that handles networking, authentication, and specialized services like NFT management.

## Installation

```bash
npm install @elacity-js/api
```

## Getting Started

To begin using the API, instantiate the `ElacityClient`. By default, it connects to the Elastos (ESC) network.

```typescript
import { ElacityClient } from '@elacity-js/api';

// Initialize with default settings (Elastos Network)
const client = new ElacityClient();

// Or target a specific network
const baseClient = new ElacityClient({ chainId: 8453 }); // Base Network
```

### Supported Networks

The SDK automatically routes requests to the appropriate environment based on the `chainId`:

| Chain ID | Network | Base URL |
| :--- | :--- | :--- |
| **20** | Elastos | `https://ela.city/api` |
| **8453** | Base | `https://base.ela.city/api` |
| **421614** | Arbitrum Sepolia (Staging) | `https://staging.ela.city/api` |

## Core Components

- [[Authentication]]: Learn how to handle SIWE login and session management.
- [[NFT-Service]]: Explore queries for NFT listings, metadata, and asset retrieval.
- [[Collection-Service]]: Discover and retrieve detailed information about NFT collections.
- [[Channel-Service]]: Manage and discover Elacity Channels and subscription plans.

## Advanced Configuration

You can override the base URL manually if needed:

```typescript
const customClient = new ElacityClient({
  baseUrl: 'https://my-custom-endpoint.com/api'
});
```
