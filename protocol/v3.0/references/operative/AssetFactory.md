# AssetFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/operative/AssetFactory.sol)

**Inherits:**
[IDigitalAssetCreator](/contracts/operative/IDigitalAssetCreator.md), Initializable, OwnableUpgradeable, [ContractIntrospector](/contracts/modules/library/ContractIntrospector.md)

**Title:**
AssetFactory

Channel-facing orchestrator for operative creation and post-mint asset wiring.
Mirrors the router model used by `ChannelFactory` by selecting operative factories via
owner-managed `opType -> factory` registrations.


## State Variables
### cstore
Shared ecosystem storage contract.


```solidity
IStorage public cstore
```


### factories
Mapping: operative type => operative factory address.


```solidity
mapping(uint16 => address) public factories
```


## Functions
### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() ;
```

### initialize

Initializes the creator router with central storage reference.


```solidity
function initialize(address _storage) external initializer;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_storage`|`address`|Central storage contract.|


### setDataStorage

Updates the central storage contract reference.


```solidity
function setDataStorage(address _storage) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_storage`|`address`|Central storage contract.|


### registerFactory

Registers or updates the operative factory for a given operative type.


```solidity
function registerFactory(uint16 opType, address factoryAddr) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`opType`|`uint16`|Operative type selector.|
|`factoryAddr`|`address`|Operative factory implementing `IOperativeFactory`.|


### registerNewAsset

Registers a newly minted asset in the protection/trading system.


```solidity
function registerNewAsset(
    address authority,
    address owner,
    address ledger,
    uint256 tokenId,
    uint16 opType,
    bytes calldata opRawData,
    bytes calldata sellRawData
) external whitelistOnly(cstore) returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authority`|`address`|Authority contract orchestrating downstream modules.|
|`owner`|`address`|Asset owner.|
|`ledger`|`address`||
|`tokenId`|`uint256`|Minted token id.|
|`opType`|`uint16`|Operative type id.|
|`opRawData`|`bytes`|Encoded operative deployment/config data. For `opType == 0`, this must still encode at least the leading `bytes16 contentId` so IP can be bound even when no operative is deployed.|
|`sellRawData`|`bytes`|Encoded listing data.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the created operative contract, or zero if none.|


### _createAndWireOperative

Creates a new operative and wires it to the ledger.


```solidity
function _createAndWireOperative(
    address owner,
    address channel,
    uint256 tokenId,
    uint16 opType,
    bytes16 contentId,
    bytes calldata opRawData
) internal returns (address operative);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`owner`|`address`|Address that will own the new operative.|
|`channel`|`address`|Address of the ledger to which the operative will be wired.|
|`tokenId`|`uint256`|Token ID of the operative.|
|`opType`|`uint16`|Operative type selector.|
|`contentId`|`bytes16`||
|`opRawData`|`bytes`|Raw data for the operative.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`operative`|`address`|Address of the newly-deployed operative.|


### _listAccess

Lists access for a given asset.


```solidity
function _listAccess(
    address authority,
    address owner,
    address ledger,
    uint256 tokenId,
    address operative,
    bytes calldata sellRawData
) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authority`|`address`|Address of the authority to which the access will be listed.|
|`owner`|`address`|Address that will own the new operative.|
|`ledger`|`address`|Address of the ledger to which the operative will be wired.|
|`tokenId`|`uint256`|Token ID of the operative.|
|`operative`|`address`|Address of the operative to be listed.|
|`sellRawData`|`bytes`|Raw data for the operative.|


### _setDataStorage

Updates the central storage contract reference.


```solidity
function _setDataStorage(address _cstore) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_cstore`|`address`|Central storage contract.|


### _extractContentId

Extracts the content ID from the operative data.


```solidity
function _extractContentId(bytes calldata opRawData) internal pure returns (bytes16 contentId);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`opRawData`|`bytes`|Raw data for the operative.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`contentId`|`bytes16`|Content ID of the operative.|


## Events
### OperativeFactoryRegistered
Emitted when an operative factory is registered or updated.


```solidity
event OperativeFactoryRegistered(uint16 indexed opType, address indexed factoryAddr);
```

### DataStorageUpdated
Emitted when storage reference is updated.


```solidity
event DataStorageUpdated(address indexed previousStorage, address indexed newStorage);
```

## Errors
### InvalidAddress
Thrown when an address argument is zero.


```solidity
error InvalidAddress(address needle);
```

### OperativeFactoryNotRegistered
Thrown when no operative factory exists for `opType`.


```solidity
error OperativeFactoryNotRegistered(uint16 opType);
```

### InvalidOperativeData
Thrown when `opRawData` cannot provide at least the leading `bytes16 contentId`.


```solidity
error InvalidOperativeData();
```

### MissingOperativeForListing
Thrown when sell data is supplied but no operative is created.


```solidity
error MissingOperativeForListing();
```

