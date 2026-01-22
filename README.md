# Elacity JS SDK Documentation

Welcome to the official documentation for Elacity's JavaScript SDKs. Use these tools to integrate Elacity's backend services, manage NFTs, handle decentralized authentication, and build rich media experiences.

## Overview

The Elacity SDK suite is designed to be modular, type-safe, and easy to integrate into any modern JavaScript or TypeScript environment.

### Core Utility Package: `@elacity-js/core`

Common types, interfaces, and utilities used across the SDK:
*   **Authentication Flow**: Shared `AuthUser` and `AuthSigner` interfaces.
*   **Pagination**: Global types for handling large data sets.
*   **Storage**: Shared interfaces for session persistence.

### API Package: `@elacity-js/api`

## Quick Start

To get started with our core API package:

1.  **Installation**:
    ```bash
    npm install @elacity-js/api @elacity-js/core ethers
    ```

2.  **Initialize the Client**:
    ```typescript
    import { ElacityClient } from '@elacity-js/api';

    // Connect to Base (Chain ID: 8453) by default
    const client = new ElacityClient();
    ```

3.  **Explore the Documentation**:
    *   [Installation Guide](api/getting-started/installation.md)
    *   [Authentication & SIWE](api/getting-started/authentication.md)
    *   [NFT Services](api/services/nfts.md)

---

## Technical Details

The SDK automatically routes requests to the appropriate environment based on the network you target:

| Chain ID | Network | API Environment |
| :--- | :--- | :--- |
| **20** | Elastos | Production |
| **8453** | Base | Production |
| **421614** | Arbitrum Sepolia | Staging |

For more advanced configurations, including custom base URLs or storage implementations, see our [Full API Reference](api/getting-started/installation.md).
