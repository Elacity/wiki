# IPTracker
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/storage/IPTracker.sol)

**Inherits:**
[IIPTracker](/contracts/storage/IIPTracker.md), [ContractIntrospector](/contracts/modules/library/ContractIntrospector.md)

**Title:**
IPTracker

Tracks the operative contract responsible for each `(channel, tokenId)` asset pair.


## State Variables
### IP_TRACKER_STORAGE_LOCATION

```solidity
bytes32 private constant IP_TRACKER_STORAGE_LOCATION =
    0xc5f11b28f74a332ab801393c8285dfd437d4b09c86e3bca10566e2497ad6ba00
```


## Functions
### _getIpTrackerModuleStorage

Retrieves the storage reference for the IPMapperModule.

Uses a custom storage location according to ERC-7201 standard to prevent storage collisions.


```solidity
function _getIpTrackerModuleStorage() private pure returns (IpTrackerModuleStorage storage $);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`$`|`IpTrackerModuleStorage`|A storage reference to IpTrackerModuleStorage.|


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


### bindIP

Binds a unique content ID to a specific token belonging to a channel regardless of the scope of the token whether it's public or not.

Reverts if the content ID is already bound mapping the contentId to the operative in charge of the IP.


```solidity
function bindIP(bytes16 _contentId, address channel, uint256 tokenId)
    external
    whitelistOnly(ISystemTracker(address(this)));
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contentId`|`bytes16`|The 16-byte content ID to map.|
|`channel`|`address`|The address of the channel that the IP is bound to.|
|`tokenId`|`uint256`|The unique identifier of the token that the IP is bound to.|


### registerDigitalAsset

Regsiter a Digital Asset item and bind it to an Operative contract


```solidity
function registerDigitalAsset(address channel, uint256 tokenId, address op)
    external
    whitelistOnly(ISystemTracker(address(this)));
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|address - Address of the NFT contract that holds registy of assets|
|`tokenId`|`uint256`|uint256 - Token ID of the asset|
|`op`|`address`|address - Address of operative contract|


### ipReference

Retrieves the channel and token ID bound to a specific content ID.

Useful for querying the tracking details of mapped IP.


```solidity
function ipReference(bytes16 _contentId) public view returns (address channel, uint256 tokenId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contentId`|`bytes16`|The 16-byte content ID to get the IP tracking for.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`address`|The address of the channel that the IP is bound to.|
|`tokenId`|`uint256`|The identifier of the token that the IP is bound to.|


## Structs
### IpTrackerModuleStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.IPTrackerModule


```solidity
struct IpTrackerModuleStorage {
    /**
     * @notice Channel => tokenId => operative contract address.
     */
    mapping(address => mapping(uint256 => address)) operator;

    /**
     * @dev map the contentId to the operative in charge of the IP
     */
    mapping(bytes16 => IpTrack) ipBindings;
}
```

