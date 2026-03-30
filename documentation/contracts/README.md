# Contracts SDK Documentation

The `@elacity-js/contracts` package provides framework-agnostic TypeScript wrappers for Elacity smart contracts, with adapters for Ethers and Viem.

This section is dedicated to SDK and client integration. Protocol architecture and contract references live under `protocol/`.

> Maintainer policy: any change under `packages/contracts/**` must include matching updates in this section.

## Start Here

- [SDK Installation](sdk/installation.md)
- [Transaction Handling](sdk/transactions.md)
- [Universal Account Executor](sdk/universal-account-executor.md)

## SDK Contract Wrappers

- [AuthorityGateway](sdk/authority.md)
- [EventHub](sdk/event-hub.md)
- [CentralStorage](sdk/central-storage.md)
- [ChannelFactory](sdk/channel-factory.md)
- [RoyaltyTradeGateway](sdk/royalty-trade-gateway.md)
- [CoreStorage](sdk/core-storage.md) (legacy alias; prefer CentralStorage on v3)
- [ChannelCore](sdk/channel-core.md) (legacy alias; prefer ChannelFactory on v3)
- [TradeGateway](sdk/trade-gateway.md)
- [StandardChannel](sdk/channel.md)
- [MultiChannel](sdk/multi-channel.md)
- [Operatives](sdk/operative.md)

## Protocol Documentation

- [Protocol v3.0](../../protocol/v3.0/README.md) (current architecture and references)
- [Protocol v2.0](../../protocol/v2.0/README.md) (historical compatibility references)

## Installation

```bash
npm install @elacity-js/contracts
npm install @elacity-js/contracts-ethers-adapter ethers
# or
npm install @elacity-js/contracts-viem-adapter viem
```

## Usage

```typescript
import { EthersAdapter } from '@elacity-js/contracts-ethers-adapter';
import { AuthorityGateway, setupContracts } from '@elacity-js/contracts';
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider('https://rpc-evm.ela.city');
const adapter = new EthersAdapter(provider);

setupContracts({ version: '3.0' }); // optional, defaults to 3.0

const gateway = new AuthorityGateway('0x...', adapter);
const pending = await gateway.sellAccess(
  ledgerAddress,
  tokenId,
  quantity,
  pricePerToken,
  payToken
);

const tx = await pending.commit();
await tx.wait();
```

## Versioning Notes

- SDK defaults to ecosystem version `3.0`.
- Ecosystem wrappers are version-aware (`CentralStorage`, `ChannelFactory`, `RoyaltyTradeGateway`, `AuthorityGateway`, `EventHub`).
- Legacy aliases remain available (`CoreStorage`, `ChannelCore`, `TradeGateway`) for compatibility.
- Shared module/token wrappers use the v3 ABI set.
