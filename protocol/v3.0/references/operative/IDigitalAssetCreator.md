# IDigitalAssetCreator
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/operative/IDigitalAssetCreator.sol)

**Title:**
DigitalAssetCreator

Creates operative/listing state for a newly minted asset.


## Functions
### registerNewAsset

Registers a newly minted asset in the protection/trading system.


```solidity
function registerNewAsset(
    address authority,
    address owner,
    address channel,
    uint256 tokenId,
    uint16 opType,
    bytes calldata opRawData,
    bytes calldata sellRawData
) external returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authority`|`address`|Authority contract orchestrating downstream modules.|
|`owner`|`address`|Asset owner.|
|`channel`|`address`|Asset channel contract the media belongs to.|
|`tokenId`|`uint256`|Minted token id.|
|`opType`|`uint16`|Operative type id.|
|`opRawData`|`bytes`|Encoded operative deployment/config data. For `opType == 0`, this must still encode at least the leading `bytes16 contentId` so IP can be bound even when no operative is deployed.|
|`sellRawData`|`bytes`|Encoded listing data.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the created operative contract, or zero if none.|


## Events
### DigitalAssetRegistered
Emitted after a new asset is processed through this orchestrator.


```solidity
event DigitalAssetRegistered(
    address indexed ledger, uint256 indexed tokenId, address indexed operative, uint16 opType, bytes16 contentId
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|Address of the NFT contract that holds registy of assets|
|`tokenId`|`uint256`|Token ID of the asset|
|`operative`|`address`|Address of operative contract that holds the logic of the asset distribution|
|`opType`|`uint16`|Operative type id.|
|`contentId`|`bytes16`|Content id that already has a mapping.|

