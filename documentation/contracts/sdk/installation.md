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
import { ChainId } from '@elacity-js/common';
import {
  ChannelFactory,
  CentralStorage,
  RoyaltyTradeGateway,
  StandardChannel,
  setupContracts,
} from '@elacity-js/contracts';
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider('https://rpc-evm.ela.city');
const adapter = new EthersAdapter(provider);

// Configure once during app startup (default is '3.0')
setupContracts({ version: '3.0' });

// Ecosystem contracts have a fixed address per supported network (recommended):
const chainId = ChainId.Base;
const centralStorage = CentralStorage.fromChainId(chainId, adapter);
const channelFactory = ChannelFactory.fromChainId(chainId, adapter);
const royaltyTradeGateway = RoyaltyTradeGateway.fromChainId(chainId, adapter);

const contract = new StandardChannel('0x...', adapter);
const balance = await contract.balanceOf('0x...');
console.log('Balance:', balance.toString());
```

### With Viem

```typescript
import { ViemAdapter } from '@elacity-js/contracts-viem-adapter';
import { ChainId } from '@elacity-js/common';
import {
  ChannelFactory,
  CentralStorage,
  RoyaltyTradeGateway,
  StandardChannel,
  setupContracts,
} from '@elacity-js/contracts';
import { createPublicClient, http } from 'viem';
import { mainnet } from 'viem/chains';

const publicClient = createPublicClient({
  chain: mainnet,
  transport: http(),
});

const adapter = new ViemAdapter(publicClient);

// Configure once during app startup (default is '3.0')
setupContracts({ version: '3.0' });

// Ecosystem contracts have a fixed address per supported network (recommended):
const chainId = ChainId.Base;
const centralStorage = CentralStorage.fromChainId(chainId, adapter);
const channelFactory = ChannelFactory.fromChainId(chainId, adapter);
const royaltyTradeGateway = RoyaltyTradeGateway.fromChainId(chainId, adapter);

const contract = new StandardChannel('0x...', adapter);
const balance = await contract.balanceOf('0x...');
console.log('Balance:', balance.toString());
```

## Ecosystem fixed-address contracts

Some contracts are **singletons per network** (one deployment per supported chainId) and their addresses are provided by the SDK.

Currently:
- `ChannelFactory` (`ChannelCore` compatibility alias)
- `CentralStorage` (`CoreStorage` compatibility alias)
- `RoyaltyTradeGateway` (`TradeGateway` compatibility alias)
- `EventHub` (v3 only)
- `AuthorityGateway` (version-aware ABI, explicit address instantiation in current SDK wrapper)

### Contract Version Setup

Configure the contract/ecosystem version during setup:

```typescript
import { setupContracts, setupContractClient, getContractVersion } from '@elacity-js/contracts';

setupContracts({ version: '3.0' }); // default
// or
setupContractClient({ version: '2.0' });

console.log(getContractVersion());
```

Versioning behavior:
- Only ecosystem contracts are version-aware.
- Canonical v3 names: `CentralStorage`, `ChannelFactory`, `RoyaltyTradeGateway`.
- Compatibility aliases: `CoreStorage`, `ChannelCore`, `TradeGateway`.
- Standard/common wrappers (e.g. `ERC20`, `ERC1155`, `StandardChannel`, `MultiChannel`, `Operative*`) always use v3 ABIs.
- For ecosystem addresses, `2.0` keeps existing addresses.
- `3.0` currently has Base + Arbitrum Sepolia entries only. Elastos has no `3.0` entry.

You can instantiate them with `fromChainId(chainId, runner)`, or use `getEcosystemContractAddress(chainId, key)` if you only need the address.

### ABI Helpers (Advanced)

```typescript
import { getContractAbi, getContractAbiForVersion } from '@elacity-js/contracts';

// Uses active setupContracts() version
const activeAbi = getContractAbi('AuthorityGateway');

// Explicit version selection
const v2Abi = getContractAbiForVersion('TradeGateway', '2.0');
const v3Abi = getContractAbiForVersion('RoyaltyTradeGateway', '3.0');
```

`getContractAbi(contract)` is intentionally version-implicit (uses active SDK setup).  
Use `getContractAbiForVersion(contract, version)` for deterministic explicit lookup.

## Available Contracts

### Core Contracts
- `ChannelFactory`: Canonical v3 channel entrypoint.
- `CentralStorage`: Canonical v3 storage entrypoint.
- `EventHub`: v3 event-router for centralized protocol emissions.
- `ChannelCore`: Compatibility alias of `ChannelFactory`.
- `CoreStorage`: Compatibility alias of `CentralStorage`.

### Channel Contracts
- `StandardChannel`: Elacity Standard Channel contract (can be Public or Private). Combines ERC-1155 functionality with subscription management. Supports minting.
- `MultiChannel`: A channel type that supports linking to other existing channels. Does not support minting.

### Gateway Contracts
- `AuthorityGateway`: Access control, licensing, and access token marketplace.
- `RoyaltyTradeGateway`: Canonical v3 marketplace for royalty-token trading.
- `TradeGateway`: Compatibility alias of `RoyaltyTradeGateway`.

### Asset Contracts
- `Operative`: Specialized ERC-1155 contracts that manage access rights and royalty distribution for digital assets.

### Standard Token Interfaces
- `ERC20`, `ERC721`, `ERC1155`: Standard token interfaces.

### Subscription Features
- Subscription methods are exposed directly on channel wrappers (`StandardChannel`, `MultiChannel`) rather than a standalone `SubscriptionModule` SDK wrapper.

## Transaction Executors

The SDK supports pluggable transaction execution strategies via the `ITransactionExecutor` interface. This allows switching between standard (one-at-a-time) and bundled (smart account) transaction execution without changing contract wrapper code.

The executor toggles the runner's `dryRun` flag so that contract method calls return raw call data instead of submitting on-chain. The executor then collects the raw data and performs the actual submission.

### Standard Execution (default)

```typescript
import { StandardTransactionExecutor } from '@elacity-js/contracts';

const runner = new EthersAdapter(new BrowserProvider(provider));
const channel = new StandardChannel('0x...', runner);
const executor = new StandardTransactionExecutor(runner);

// With StandardTransactionExecutor, dryRun stays false.
// Contract calls execute normally — same as calling channel.mint() directly.
const response = await executor.execute([
  channel.mint(uri, opType, opRawData, sellRawData),
]);
await Promise.all(response.transactions.map((tx) => tx.wait()));
```

### Bundled Execution via Particle Universal Account

Install the UA executor package:

```bash
npm install @elacity-js/contracts-ua-executor @particle-network/universal-account-sdk
```

```typescript
import { UniversalAccountTransactionExecutor } from '@elacity-js/contracts-ua-executor';

const executor = new UniversalAccountTransactionExecutor(runner, {
  ua,
  chainId: 8453,
  signMessage: (msg) => provider.request({ method: 'personal_sign', params: [msg, eoaAddress] }),
});

// Bundle multiple contract calls into one UA meta-transaction.
// The executor toggles dryRun, collects raw calldata, then sends via UA.
const response = await executor.execute([
  channel.mint(uri, opType, opRawData, sellRawData),
  operative.setApprovalForAll(operator, true),
]);
await Promise.all(response.transactions.map((tx) => tx.wait()));
```

For more in-depth information on how transactions are handled in the SDK, please refer to:
- [**Transaction Handling**](transactions.md)
- [**Universal Account Executor**](universal-account-executor.md)
