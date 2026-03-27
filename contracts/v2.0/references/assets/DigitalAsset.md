## IDigitalAssetCreator

Creates operative/listing state for a newly minted asset.

### registerNewAsset

```solidity
function registerNewAsset(address authority, address owner, address ledger, uint256 tokenId, uint16 opType, bytes opRawData, bytes sellRawData) external returns (address)
```

Registers a newly minted asset in the protection/trading system.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| authority | address | Authority contract orchestrating downstream modules. |
| owner | address | Asset owner. |
| ledger | address | Asset collection/ledger contract. |
| tokenId | uint256 | Minted token id. |
| opType | uint16 | Operative type id. |
| opRawData | bytes | Encoded operative deployment/config data. |
| sellRawData | bytes | Encoded listing data. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of the created operative contract, or zero if none. |

## DigitalAssetCreator

Creates operative/listing state for a newly minted asset.

_Upgradeable via `reinitializer(VERSION)`. Delegates state to the shared `IStorage` contract
This contract implements `IDigitalAssetCreator` interface_

### ZeroQuantityError

```solidity
error ZeroQuantityError()
```

Thrown when listing quantity is zero.

### ZeroPriceError

```solidity
error ZeroPriceError()
```

Thrown when listing price is zero.

### registerNewAsset

```solidity
function registerNewAsset(address authority, address owner, address ledger, uint256 tokenId, uint16 opType, bytes opRawData, bytes sellRawData) external returns (address)
```

Creates operative/listing state for a newly minted asset.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| authority | address | Authority contract orchestrating trading/protection. |
| owner | address | Owner of the minted asset. |
| ledger | address | Ledger contract where the asset was minted. |
| tokenId | uint256 | Minted asset id. |
| opType | uint16 | Operative type id. |
| opRawData | bytes | Encoded operative creation data. |
| sellRawData | bytes | Encoded listing data. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of the created operative contract, or zero if skipped. |

### _sellAccessTokens

```solidity
function _sellAccessTokens(address authority, address seller, address ledger, uint256 tokenId, bytes sellRawData) internal
```

Sell access token, just extract inputs from raw data and call sellAccessOnBehalf

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| authority | address | address - Authority contract address |
| seller | address | Address listing access tokens. |
| ledger | address | Asset ledger address. |
| tokenId | uint256 | uint256 - Token id to sell |
| sellRawData | bytes | bytes - Encoded listing tuple `(quantity, price, paymentToken)`. |

### _issueOperativeTokens

```solidity
function _issueOperativeTokens(address creator, uint16 opType, bytes data) internal returns (address)
```

Creates an operative contract and mints its initial control/access tokens.
token dispatch are all handled in the contract design

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| creator | address | address - creator address (needed for contract ownership) |
| opType | uint16 | uint16 - Operative type, define what is the flow to adopt |
| data | bytes | bytes - Encoded operative bootstrap payload. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of the new Operative contract, will be zero-address if no operative created (case of free content) |

### _getOperativeFactory

```solidity
function _getOperativeFactory(uint16 _opType) internal view virtual returns (address)
```

Internal helper returning the factory address for an operative type.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _opType | uint16 | Operative type id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Factory address mapped to `_opType`. |

## DigitalAsset

Abstract base contract for digital asset management, mostly implemented by standard
channel-type contracts

_Implements `IDigitalAsset` interface._

### _registerProtectiveFlow

```solidity
function _registerProtectiveFlow(address authority, address registry, uint256 tokenId, uint16 opType, bytes opRawData, bytes sellRawData) internal returns (address)
```

Register asset onto the protection system.

No further action is operated for unencrypted content.

For encrypted content, it will assign a new Operative contract and make
all the initial setup of royalty distribution to th asset-level and other
tokens to manage access to the individual media asset.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| authority | address | Address of the contract in charge of all protective flows |
| registry | address | Address of the contract registry. |
| tokenId | uint256 | ID of the asset |
| opType | uint16 | Operative type, define what is the flow to adopt |
| opRawData | bytes | Operative raw data, "0x" to omit operative contract creation |
| sellRawData | bytes | Sell raw data, "0x" to omit listing step |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of created operative contract, or zero if none. |

### _registerTransferAuthorization

```solidity
function _registerTransferAuthorization(address op, address authority, address tradeGateway) internal
```

Grants transfer permissions on operative tokens to trusted protocol contracts.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| op | address | Operative contract address. |
| authority | address | Authority contract to authorize. |
| tradeGateway | address | Trade gateway address (reserved for optional authorization). |

