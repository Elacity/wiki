# IDigitalAsset
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/assets/IDigitalAsset.sol)

**Title:**
IDigitalAsset

Interface for the digital asset contract.


## Functions
### mint

Mints a digital asset and optionally initializes its protection/trading flow.


```solidity
function mint(string memory _uri, uint16 opType, bytes calldata opRawData, bytes calldata sellRawData)
    external
    payable;
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
function tokenURI(uint256 tokenId) external view returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`uint256`|Asset id.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|URI string for the token metadata.|


## Events
### DigitalAssetRegistered
Emitted when a protected asset is linked to an operative contract.


```solidity
event DigitalAssetRegistered(address indexed ledger, uint256 indexed tokenId, address indexed operator);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|Channel contract address hosting the asset.|
|`tokenId`|`uint256`|Newly created asset id.|
|`operator`|`address`|Operative contract handling protected access.|

### AssetCreated
Emitted when a new asset is minted, encrypted or unencrypted.


```solidity
event AssetCreated(
    address indexed _to, uint256 indexed _tokenId, string _tokenUri, uint16 _opType, address indexed opContract
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_to`|`address`|Asset beneficiary.|
|`_tokenId`|`uint256`|Asset identifier.|
|`_tokenUri`|`string`|Metadata URI of the json-formatted metadata of the asset.|
|`_opType`|`uint16`|Operative type id (`0` for unencrypted assets).|
|`opContract`|`address`|Operative contract address, or zero when none is created.|

