# API Reference

The `@elacity-js/api` package provides a REST and GraphQL client for interacting with Elacity's backend services across multiple blockchain networks.

## Overview

The API SDK automatically routes requests to the appropriate environment based on the network you target:

| Chain ID | Network | API Environment |
| :--- | :--- | :--- |
| **20** | Elastos | Production |
| **8453** | Base | Production |
| **421614** | Arbitrum Sepolia | Staging |

## Disclaimer

> This API sdk doesn't cover a comprehensive set of all the endpoints from the REST and GraphQL, it only implements most frequently used endpoints to achieve most common workflows.
> You can find a comprenshive backend implementation in [API REST Reference](../../references/README.md) and [GraphQL Reference](../../graphql/README.md).
> Please file an issue to request and extend support of any existing endpoints.

### Quick Start

1.  **Installation**:
    ```bash
    npm install @elacity-js/api @elacity-js/common ethers
    ```

2.  **Initialize the Client**:
    ```typescript
    import { ElacityClient } from '@elacity-js/api';

    // Connect to Base (Chain ID: 8453) by default
    const client = new ElacityClient();
    ```

3.  **Explore the Documentation**:
    For detailed guides on installation, authentication, and using services, see the sections below.

---

## Getting Started
* [Installation](getting-started/installation.md)
* [Authentication](getting-started/authentication.md)
* [Dataset and Pagination](getting-started/dataset-and-pagination.md)
* [Errors and Retries](getting-started/errors-and-retries.md)

## Services
* [Account & Identity](services/identity.md)
* [Channels](services/channel.md)
* [NFTs](services/nfts.md)
* [Collections](services/collection.md)
* [Background Jobs](services/background-job.md)
