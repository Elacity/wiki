# Contracts Package

The `@elacity-js/contracts` package provides framework-agnostic wrappers for Elacity smart contracts. It uses an adapter pattern to support different web3 libraries like `ethers` and `viem`.

## Installation

```bash
npm install @elacity-js/contracts
```

You will also need to install one of the adapters and its corresponding peer dependency:

### Using Ethers.js
```bash
npm install @elacity-js/contracts-ethers-adapter ethers
```

### Using Viem
```bash
npm install @elacity-js/contracts-viem-adapter viem
```

## Basic Usage

### With Ethers.js

```typescript
import { EthersAdapter } from '@elacity-js/contracts-ethers-adapter';
import { ChainId } from '@elacity-js/core';
import { ChannelCore, CoreStorage, StandardChannel, TradeGateway } from '@elacity-js/contracts';
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider('https://rpc-evm.ela.city');
const adapter = new EthersAdapter(provider);

// Ecosystem contracts have a fixed address per supported network (recommended):
const chainId = ChainId.Base;
const coreStorage = CoreStorage.fromChainId(chainId, adapter);
const channelCore = ChannelCore.fromChainId(chainId, adapter);
const tradeGateway = TradeGateway.fromChainId(chainId, adapter);

const contract = new StandardChannel('0x...', adapter);
const balance = await contract.balanceOf('0x...');
console.log('Balance:', balance.toString());
```

### With Viem

```typescript
import { ViemAdapter } from '@elacity-js/contracts-viem-adapter';
import { ChainId } from '@elacity-js/core';
import { ChannelCore, CoreStorage, StandardChannel, TradeGateway } from '@elacity-js/contracts';
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const publicClient = createPublicClient({
  chain: mainnet,
  transport: http(),
});

const adapter = new ViemAdapter(publicClient);

// Ecosystem contracts have a fixed address per supported network (recommended):
const chainId = ChainId.Base;
const coreStorage = CoreStorage.fromChainId(chainId, adapter);
const channelCore = ChannelCore.fromChainId(chainId, adapter);
const tradeGateway = TradeGateway.fromChainId(chainId, adapter);

const contract = new StandardChannel('0x...', adapter);
const balance = await contract.balanceOf('0x...');
console.log('Balance:', balance.toString());
```

## Ecosystem fixed-address contracts

Some contracts are **singletons per network** (one deployment per supported chainId) and their addresses are provided by the SDK.

Currently:
- `ChannelCore`
- `CoreStorage`
- `TradeGateway`

Supported chain IDs are exported as `ChainId` (e.g. `ChainId.Base`, `ChainId.ArbitrumSepolia`, `ChainId.Elastos`).

You can instantiate them with `fromChainId(chainId, runner)`, or use `getEcosystemContractAddress(chainId, key)` if you only need the address.

## Available Contracts

### Core Contracts
- `ChannelCore`: Factory contract for creating and managing Elacity channels. Handles channel creation through registered factories.
- `CoreStorage`: Central storage contract for ecosystem data including IP bindings, channel relationships, marketplace listings, and system configuration.

### Channel Contracts
- `StandardChannel`: Elacity Standard Channel contract (can be Public or Private). Combines ERC-1155 functionality with subscription management. Supports minting.
- `MultiChannel`: A channel type that supports linking to other existing channels. Does not support minting.

### Gateway Contracts
- `AuthorityGateway`: Access control, licensing, and access token marketplace.
- `TradeGateway`: Marketplace dedicated to trading royalty tokens (ROYALTY_SHARE tokens from Operative contracts).

### Asset Contracts
- `Operative`: Specialized ERC-1155 contracts that manage access rights and royalty distribution for digital assets.

### Standard Token Interfaces
- `ERC20`, `ERC721`, `ERC1155`: Standard token interfaces.

### Module Contracts
- `SubscriptionModule`: Core subscription management module (inherited by all channel types).
