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
import { DigitalAsset } from '@elacity-js/contracts';
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider('https://rpc-evm.ela.city');
const adapter = new EthersAdapter(provider);

const contract = new DigitalAsset('0x...', adapter);
const balance = await contract.balanceOf('0x...');
console.log('Balance:', balance.toString());
```

### With Viem

```typescript
import { ViemAdapter } from '@elacity-js/contracts-viem-adapter';
import { DigitalAsset } from '@elacity-js/contracts';
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const publicClient = createPublicClient({
  chain: mainnet,
  transport: http(),
});

const adapter = new ViemAdapter(publicClient);

const contract = new DigitalAsset('0x...', adapter);
const balance = await contract.balanceOf('0x...');
console.log('Balance:', balance.toString());
```

## Available Contracts

- `DigitalAsset`: Elacity Channel contract (can be Public or Private).
- `TradeGateway`: Marketplace interactions.
- `AuthorityGateway`: Access control and licensing.
- `MultiChannel`: A channel type that supports linking to other existing channels.
- `SubscriptionModule`: Core subscription management module (inherited by all channel types).
- `ERC20`, `ERC721`, `ERC1155`: Standard token interfaces.
