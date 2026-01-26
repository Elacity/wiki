## IDigitalAssetCreator

### registerNewAsset

```solidity
function registerNewAsset(address authority, address owner, address ledger, uint256 tokenId, uint16 opType, bytes opRawData, bytes sellRawData) external returns (address)
```

## DigitalAssetCreator

### ZeroQuantityError

```solidity
error ZeroQuantityError()
```

### ZeroPriceError

```solidity
error ZeroPriceError()
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(address s) public
```

### registerNewAsset

```solidity
function registerNewAsset(address authority, address owner, address ledger, uint256 tokenId, uint16 opType, bytes opRawData, bytes sellRawData) external returns (address)
```

### _sellAccessTokens

```solidity
function _sellAccessTokens(address authority, address seller, address ledger, uint256 tokenId, bytes sellRawData) internal
```

Sell access token, just extract inputs from raw data and call sellAccessOnBehalf

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| authority | address | address - Authority contract address |
| seller | address |  |
| ledger | address |  |
| tokenId | uint256 | uint256 - Token id to sell |
| sellRawData | bytes | bytes - Raw data to parse, supposed to contains quantity, price and pay token |

### _issueOperativeTokens

```solidity
function _issueOperativeTokens(address creator, uint16 opType, bytes data) internal returns (address)
```

Create operative contract and mint all tokens supposed to hendled inside of it,
token dispatch are all handled in the contract design

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| creator | address | address - creator address (needed for contract ownership) |
| opType | uint16 | uint16 - Operative type, define what is the flow to adopt |
| data | bytes | bytes - Raw data to parse, supposed to contains all tokens dispatch |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of the new Operative contract, will be Zero-Address if not operative created (case of free content) |

### _getOperativeFactory

```solidity
function _getOperativeFactory(uint16 _opType) internal view virtual returns (address)
```

Intrnal method to retrieve the factory contract related to the operative type

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _opType | uint16 | Type of the target operative |

## DigitalAsset

### __DigitalAsset_init

```solidity
function __DigitalAsset_init() internal
```

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
| registry | address |  |
| tokenId | uint256 | ID of the asset |
| opType | uint16 | Operative type, define what is the flow to adopt |
| opRawData | bytes | Operative raw data, "0x" to omit operative contract creation |
| sellRawData | bytes | Sell raw data, "0x" to omit listing step |

### _registerTransferAuthorization

```solidity
function _registerTransferAuthorization(address op, address authority, address tradeGateway) internal
```

