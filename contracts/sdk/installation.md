# SDK Installation and Quick Setup

This section documents the V3-oriented SDK setup patterns.

## Install

```bash
npm install @elacity-js/contracts
```

Install your preferred adapter:

```bash
npm install @elacity-js/contracts-ethers-adapter ethers
# or
npm install @elacity-js/contracts-viem-adapter viem
```

## Core wrappers to use in V3 docs

- `AuthorityGateway`
- `RoyaltyTradeGateway`
- `ChannelFactory`
- `CentralStorage`
- Channel wrappers (`DigitalAssetPublic`, `DigitalAssetPrivate`, `MultiChannel`)
- Operative wrappers (`OperativeBuyable`, `OperativeBuyableSellable`)

## Basic setup example (ethers)

```typescript
import { EthersAdapter } from '@elacity-js/contracts-ethers-adapter';
import { JsonRpcProvider } from 'ethers';

const provider = new JsonRpcProvider(process.env.RPC_URL!);
const adapter = new EthersAdapter(provider);
```

## Contract initialization example

```typescript
import {
  AuthorityGateway,
  RoyaltyTradeGateway,
  ChannelFactory,
  CentralStorage,
} from '@elacity-js/contracts';

const authority = new AuthorityGateway(process.env.AUTHORITY_GATEWAY!, adapter);
const royaltyTrade = new RoyaltyTradeGateway(process.env.ROYALTY_TRADE_GATEWAY!, adapter);
const channelFactory = new ChannelFactory(process.env.CHANNEL_FACTORY!, adapter);
const storage = new CentralStorage(process.env.CENTRAL_STORAGE!, adapter);
```

## Related pages

- [Authority Gateway](authority.md)
- [Royalty Trade Gateway](royalty-trade-gateway.md)
- [Channel Factory](channel-factory.md)
- [Central Storage](central-storage.md)
- [Channel Wrappers](channel.md)
- [MultiChannel](multi-channel.md)
- [Operatives](operative.md)
- [Transaction Handling](transactions.md)
