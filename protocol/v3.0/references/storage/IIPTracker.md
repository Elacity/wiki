# IIPTracker
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/storage/IIPTracker.sol)

**Title:**
IIPTracker

Interface for tracking intellectual properties.

Tracks which operative contract controls a given `(channel, tokenId)` pair.


## Functions
### bindIP

Binds a content id to a `(channel, tokenId)` pair.


```solidity
function bindIP(bytes16 _contentId, address channel, uint256 tokenId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contentId`|`bytes16`|Content id to bind.|
|`channel`|`address`|Channel contract address.|
|`tokenId`|`uint256`|Token id inside the channel.|


### ipReference

Returns the token reference mapped to a content id.


```solidity
function ipReference(bytes16 _contentId) external view returns (address channel, uint256 tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contentId`|`bytes16`|Content id to resolve.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Channel contract address.|
|`tokenId`|`uint256`|Token id inside the channel.|


### operator

Returns the operative address controlling an asset.


```solidity
function operator(address channel, uint256 tokenId) external view returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Channel/ledger contract address.|
|`tokenId`|`uint256`|Asset token id.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Operative contract address.|


### registerDigitalAsset

Registers a digital asset to an operative contract.

**Note:**
docs-ignore: true


```solidity
function registerDigitalAsset(address channel, uint256 tokenId, address operator) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Channel/ledger contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`operator`|`address`|Operative contract address.|


## Events
### IPBound
Emitted when a content id is mapped to a token reference.


```solidity
event IPBound(bytes16 indexed contentId, address indexed channel, uint256 indexed tokenId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contentId`|`bytes16`|Bound content id.|
|`channel`|`address`|Channel contract address.|
|`tokenId`|`uint256`|Token id inside the channel.|

## Errors
### IPAlreadyBound
Thrown when attempting to bind an already-mapped content id.


```solidity
error IPAlreadyBound(bytes16 contentId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`contentId`|`bytes16`|Content id that already has a mapping.|

### DigitalAssetAlreadyRegistered
Thrown when attempting to register an already-registered `(channel, tokenId)` operative binding.


```solidity
error DigitalAssetAlreadyRegistered(address channel, uint256 tokenId, address operator);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Channel/ledger contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`operator`|`address`|Existing operative contract already registered for this asset.|

## Structs
### IpTrack
Canonical identifier for a tracked digital asset.


```solidity
struct IpTrack {
    address channel;
    uint256 tokenId;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|Channel contract address.|
|`tokenId`|`uint256`|Token id inside the channel.|

