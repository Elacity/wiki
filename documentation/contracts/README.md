# Contracts - Smart Contract SDK

The `@elacity-js/contracts` package provides framework-agnostic TypeScript wrappers for the Elacity DRM smart contract ecosystem. Built with an adapter pattern, it supports multiple web3 libraries including Ethers.js and Viem.

## What's Inside

This section of the documentation covers the Elacity smart contract ecosystem and its JavaScript SDK wrapper.

### 📚 Getting Started

New to Elacity contracts? Start here:

- **[Architecture & Design Proposal](getting-started/design-proposal.md)** - Comprehensive overview of the DRM contract ecosystem, including vision, specifications, and rationale
- **[Ecosystem Overview](getting-started/ecosystem-overview.md)** - High-level architecture, core components, and interaction flows
- **[Installation Guide](sdk/installation.md)** - Install the SDK and choose your preferred web3 adapter

### 📖 Contract References

Auto-generated documentation for all smart contracts:

- **[Contract API Reference](../../protocol/v2.0/README.md)** - Complete API documentation for gateways, channels, operatives, modules, and storage contracts
- **[Contract Index](../../protocol/v2.0/SUMMARY.md)** - Full listing of all documented contracts

## Quick Overview

The Elacity DRM ecosystem provides:

- **🔐 Advanced Digital Rights Management** - Cryptographic licensing with ECDH/ECDSA protocols
- **💰 Multi-Stakeholder Royalties** - Complex revenue sharing inspired by EIP-2981, EIP-4910, and EIP-5553
- **🎫 Flexible Access Models** - Permanent ownership, resale rights, and subscriptions
- **🏪 Dual Marketplace System** - AuthorityGateway for access tokens, TradeGateway for asset trading
- **📦 Modular Architecture** - Reusable modules for licensing, payments, royalties, and subscriptions

## Architecture Layers

```
┌────────────────────────────────────────────┐
│     Gateway Layer (Entry Points)           │
│  AuthorityGateway  |  TradeGateway        │
├────────────────────────────────────────────┤
│     Asset Layer (Digital Content)          │
│     Channels  |  Operatives                │
├────────────────────────────────────────────┤
│     Module Layer (Specialized Logic)       │
│  License | Trade | Payment | Royalty | Sub │
├────────────────────────────────────────────┤
│     Storage Layer (Central Registry)       │
│          CoreStorage                        │
└────────────────────────────────────────────┘
```

## Key Contracts

| Contract | Purpose |
|----------|---------|
| **AuthorityGateway** | Access control, licensing, and access token marketplace |
| **TradeGateway** | General asset trading (royalty shares, distribution rights) |
| **Channels** | ERC-1155 containers for digital assets (StandardChannel, MultiChannel) |
| **Operatives** | Access control contracts for individual assets |
| **CoreStorage** | Centralized data hub for ecosystem-wide registry |

## Installation

```bash
# Install the core package
npm install @elacity-js/contracts

# Choose an adapter
npm install @elacity-js/contracts-ethers-adapter ethers
# OR
npm install @elacity-js/contracts-viem-adapter viem
```

## Usage Example

```typescript
import { EthersAdapter } from '@elacity-js/contracts-ethers-adapter';
import { AuthorityGateway } from '@elacity-js/contracts';
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider('https://rpc-evm.ela.city');
const adapter = new EthersAdapter(provider);

const gateway = new AuthorityGateway('0x...', adapter);
const commitTx = await gateway.sellAccess(ledgerAddress, tokenId, quantity, pricePerToken, payToken);

// Direct commitment
const tx = await commitTx.commit();
const receipt = await tx.wait();
```

## Transaction Handling

The SDK supports two ways to execute state-changing transactions:

1.  **Direct Commitment**: Call `.commit()` on the object returned by a contract method.
2.  **Transaction Executor**: Use an `ITransactionExecutor` (like the `UniversalAccountTransactionExecutor`) to bundle operations or handle complex execution logic.

For more details, see the [**Transaction Handling**](sdk/transactions.md) guide.

## Documentation Navigation

- Start with the [**Ecosystem Overview**](getting-started/ecosystem-overview.md) for architectural understanding
- Review the [**Design Proposal**](getting-started/design-proposal.md) for detailed specifications
- Follow the [**Installation Guide**](sdk/installation.md) to integrate the SDK
- Explore [**Contract References**](../../protocol/v2.0/README.md) for API documentation
