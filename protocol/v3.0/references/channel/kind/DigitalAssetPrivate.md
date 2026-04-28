# DigitalAssetPrivate
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/channel/kind/StandardChannel.sol)

**Inherits:**
[DigitalAssetCommon](/contracts/channel/kind/DigitalAssetCommon.md), [DigitalAsset](/contracts/modules/assets/DigitalAsset.md)

**Title:**
DigitalAssetPrivate

Private standard channel — only accounts with `MINTER_ROLE` can mint digital assets.
Deployed behind a beacon proxy by `StandardChannelPrivateFactory`.


## State Variables
### name
Human-readable name of this channel.


```solidity
string public name
```


### MINTER_ROLE
Role hash that gates the `mint` function.


```solidity
bytes32 internal constant MINTER_ROLE = keccak256("MINTER_ROLE")
```


## Functions
### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() DigitalAssetCommon();
```

### initialize

**Note:**
docs-ignore: true


```solidity
function initialize(
    address _authority,
    address _tradeGateway,
    address _registry,
    address _subscriptionManager,
    address creator,
    string memory _name,
    string memory _tokenURI,
    bytes memory _data
) public initializer;
```

### mint

Mints a digital asset and optionally initializes its protection/trading flow.


```solidity
function mint(string memory _uri, uint16 opType, bytes calldata opRawData, bytes calldata sellRawData)
    external
    payable
    onlyRole(MINTER_ROLE)
    collectAssetMintFee;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_uri`|`string`|Token metadata URI.|
|`opType`|`uint16`|Operative type id.|
|`opRawData`|`bytes`|Encoded operative initialization data (`0x` to skip).|
|`sellRawData`|`bytes`|Encoded listing data (`0x` to skip).|


### tokenURI

Returns metadata URI for a token.


```solidity
function tokenURI(uint256 tokenId) public view override(IDigitalAsset, SubscriptionModule) returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`uint256`|Asset id.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|URI string for the token metadata.|


