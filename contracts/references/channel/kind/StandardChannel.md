## DigitalAssetCommon

### TokenIdUnderflow

```solidity
error TokenIdUnderflow()
```

### TokenIdAlreadyUsed

```solidity
error TokenIdAlreadyUsed(uint256 value)
```

### authority

```solidity
address authority
```

### tradeGateway

```solidity
address tradeGateway
```

### AssetId

```solidity
struct AssetId {
  address op;
  uint256 tokenId;
}
```

### constructor

```solidity
constructor() internal
```

### initialize

```solidity
function initialize(address _authority, address _tradeGateway, address _registry, address creator, string _tokenURI, bytes _data) public
```

### _createAsset

```solidity
function _createAsset(string _tokenURI) internal returns (uint256)
```

Mint a new channel content as an unique asset

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _tokenURI | string | URI of the metadata json-formatted |

### configureChannel

```solidity
function configureChannel(bytes data) internal
```

configure the channel by parsing and applying setup

The input for `DigitalAsset` type is formatted as following `(ShareInput[], SubscriptionPlan[])`

The flow is designed as following:
 - setup initial distribution of the royalty tokens
 - setup subscription plans

Here is an example of data formatting in javascript using ethersjs

```javascript
ethers.utils.defaultAbiCoder.encode(
  [
    'tuple(address,uint256)[]',
    'tuple(uint8,address,uint256,uint256,bool)[]',
    'tuple(address,uint256)[]',
  ],
  [
    [
       // Royalties distribution
       ['0x02...12e5', 800],
       ['0x52...D17B', 200]
    ], [
       // 1 plan to create
       [0, ethers.constants.AddressZero, BigNumber.from(1).pow(18), 2592000, true]
    ],
    []
  ]
);
```

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| data | bytes | raw input to configure the channel |

### hasActiveSubscription

```solidity
function hasActiveSubscription(address account) public view returns (bool)
```

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view returns (bool)
```

## DigitalAssetPublic

### name

```solidity
string name
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(address _authority, address _tradeGateway, address _registry, address creator, string _name, string _tokenURI, bytes _data) public
```

### mint

```solidity
function mint(string _uri, uint16 opType, bytes opRawData, bytes sellRawData) external payable
```

Create a new digital asset, generates the operative contract
with initial tokens distribution and put it on sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _uri | string | string memory - Token URI |
| opType | uint16 | uint16 - Operative type, can be 0: free, 1: Buy-Play, 2: Buy-Play-Sell. refers to OP_TYPE_* constants for complete set of the type |
| opRawData | bytes | bytes calldata - Operative raw data, "0x" to omit operative contract creation |
| sellRawData | bytes | bytes calldata - Sell raw data, "0x" to omit listing step |

### tokenURI

```solidity
function tokenURI(uint256 tokenId) public view returns (string)
```

## DigitalAssetPrivate

### name

```solidity
string name
```

### MINTER_ROLE

```solidity
bytes32 MINTER_ROLE
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(address _authority, address _tradeGateway, address _registry, address creator, string _name, string _tokenURI, bytes _data) public
```

### mint

```solidity
function mint(string _uri, uint16 opType, bytes opRawData, bytes sellRawData) external payable
```

Create a new digital asset, generates the operative contract
with initial tokens distribution and put it on sale

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _uri | string | string memory - Token URI |
| opType | uint16 | uint16 - Operative type, can be 0: free, 1: Buy-Play, 2: Buy-Play-Sell. refers to OP_TYPE_* constants for complete set of the type |
| opRawData | bytes | bytes calldata - Operative raw data, "0x" to omit operative contract creation |
| sellRawData | bytes | bytes calldata - Sell raw data, "0x" to omit listing step |

### tokenURI

```solidity
function tokenURI(uint256 tokenId) public view returns (string)
```

