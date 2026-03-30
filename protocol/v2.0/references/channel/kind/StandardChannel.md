## DigitalAssetCommon

Shared base for public and private standard channels. Combines subscriptions,
royalty distribution, asset minting, and fee collection into a single upgradeable
ERC-1155 channel contract.

_Subscription lookups propagate upward through the `ChannelRegistry` to resolve
multi-channel parent subscriptions._

### InvalidTokenId

```solidity
error InvalidTokenId()
```

The generated token ID is invalid or already minted.

### authority

```solidity
address authority
```

Address of the AuthorityGateway that manages access-token trades and licensing.

### tradeGateway

```solidity
address tradeGateway
```

Address of the TradeGateway that manages token-level marketplace trades.

### _createAsset

```solidity
function _createAsset(string _tokenURI) internal returns (uint256)
```

Mints a new unique digital asset on this channel.

_The token ID is deterministically derived from `_tokenURI`. Reverts if the
generated ID is in the reserved range or has already been used._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _tokenURI | string | IPFS-based URI pointing to the asset's JSON metadata |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | The newly minted token ID |

### configureChannel

```solidity
function configureChannel(bytes data) internal
```

Applies the initial channel configuration during deployment.

Decodes `data` as `(ShareInput[], SubscriptionPlan[], TokenAccessThreshold[])` and:
  1. Mints royalty-share tokens to each beneficiary.
  2. Creates subscription plans on this channel.
  3. Registers token-ownership-based access thresholds.

_Example encoding with ethers.js:
```javascript
ethers.utils.defaultAbiCoder.encode(
  ['tuple(address,uint256)[]', 'tuple(uint8,address,uint256,uint256,bool)[]', 'tuple(address,uint256)[]'],
  [
    [['0x02...12e5', 800], ['0x52...D17B', 200]],   // royalty shares (beneficiary, share)
    [[0, ethers.constants.AddressZero, 1e18, 2592000, true]], // subscription plans
    []  // token-ownership thresholds
  ]
);
```_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| data | bytes | ABI-encoded configuration payload |

### hasActiveSubscription

```solidity
function hasActiveSubscription(address account) public view returns (bool)
```

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view returns (bool)
```

## DigitalAssetPublic

Public standard channel — any address can mint new digital assets.
Deployed behind a beacon proxy by `StandardChannelPublicFactory`.

### name

```solidity
string name
```

Human-readable name of this channel.

### mint

```solidity
function mint(string _uri, uint16 opType, bytes opRawData, bytes sellRawData) external payable
```

Mints a digital asset and optionally initializes its protection/trading flow.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _uri | string | Token metadata URI. |
| opType | uint16 | Operative type id. |
| opRawData | bytes | Encoded operative initialization data (`0x` to skip). |
| sellRawData | bytes | Encoded listing data (`0x` to skip). |

### tokenURI

```solidity
function tokenURI(uint256 tokenId) public view returns (string)
```

Returns metadata URI for a token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenId | uint256 | Asset id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | URI string for the token metadata. |

## DigitalAssetPrivate

Private standard channel — only accounts with `MINTER_ROLE` can mint digital assets.
Deployed behind a beacon proxy by `StandardChannelPrivateFactory`.

### name

```solidity
string name
```

Human-readable name of this channel.

### MINTER_ROLE

```solidity
function MINTER_ROLE() public pure returns (bytes32)
```

### mint

```solidity
function mint(string _uri, uint16 opType, bytes opRawData, bytes sellRawData) external payable
```

Mints a digital asset and optionally initializes its protection/trading flow.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _uri | string | Token metadata URI. |
| opType | uint16 | Operative type id. |
| opRawData | bytes | Encoded operative initialization data (`0x` to skip). |
| sellRawData | bytes | Encoded listing data (`0x` to skip). |

### tokenURI

```solidity
function tokenURI(uint256 tokenId) public view returns (string)
```

Returns metadata URI for a token.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenId | uint256 | Asset id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | URI string for the token metadata. |

