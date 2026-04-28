# CentralStorage

`CentralStorage` is the canonical storage wrapper for protocol `v3.0`.

It resolves to:
- v2: legacy `CoreStorage` ABI/address
- v3: `CentralStorage` ABI/address

## Import

```typescript
import { CentralStorage } from '@elacity-js/contracts';
```

## Initialization

```typescript
import { ChainId } from '@elacity-js/common';
import { setupContracts, CentralStorage } from '@elacity-js/contracts';

setupContracts({ version: '3.0' });

const storage = CentralStorage.fromChainId(ChainId.Base, adapter);
```

## Key Read Methods

```typescript
const [channel, tokenId] = await storage.ipReference(contentId);
const operative = await storage.operator(channelAddress, tokenId);
const acknowledged = await storage.acknowledged(contractAddress);
const [platformFee, feeRecipient] = await storage.taxInformation();
```

## v3-Specific Methods

```typescript
const [sharesBps, sharesRecipient] = await storage.protocolShares();
const [mediaFee, mediaFeeToken] = await storage.mediaCreationFee();
const [channelFee, channelFeeToken] = await storage.channelCreationFee();
```

## Notes

- `CentralStorage.factories(opType)` is a legacy path and unavailable in v3.
- For v3 factory routing use `ChannelFactory.factories(channelType, scope)`.
- `CoreStorage` still exists as a compatibility alias but is deprecated for v3 integrations.
