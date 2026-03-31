# DigitalAsset
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/assets/DigitalAsset.sol)

**Inherits:**
Initializable, [IDigitalAsset](../../modules/assets/IDigitalAsset.md), ContextUpgradeable

**Title:**
DigitalAsset

Abstract base contract for digital asset management, mostly implemented by standard
channel-type contracts

Implements `IDigitalAsset` interface.


## State Variables
### SLOT_ASSETCREATOR
Registry slot for the `IDigitalAssetCreator` delegate contract.


```solidity
bytes32 private constant SLOT_ASSETCREATOR = keccak256("slot.assetCreator")
```


## Functions
### __DigitalAsset_init

Initializes shared context for inheriting digital-asset contracts.

**Note:**
docs-ignore: true


```solidity
function __DigitalAsset_init() internal onlyInitializing;
```

### _registerProtectiveFlow

Register asset onto the protection system.
No further action is operated for unencrypted content.
For encrypted content, it will assign a new Operative contract and make
all the initial setup of royalty distribution to th asset-level and other
tokens to manage access to the individual media asset.


```solidity
function _registerProtectiveFlow(
    address authority,
    address registry,
    uint256 tokenId,
    uint16 opType,
    bytes calldata opRawData,
    bytes calldata sellRawData
) internal returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`authority`|`address`|Address of the contract in charge of all protective flows|
|`registry`|`address`|Address of the contract registry.|
|`tokenId`|`uint256`|ID of the asset|
|`opType`|`uint16`|Operative type, define what is the flow to adopt|
|`opRawData`|`bytes`|Operative raw data. For `opType == 0`, encode at least `bytes16 contentId` so IP can still be bound. For other operative types, "0x" omits operative creation.|
|`sellRawData`|`bytes`|Sell raw data, "0x" to omit listing step|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of created operative contract, or zero if none.|


### _registerTransferAuthorization

Grants transfer permissions on operative tokens to trusted protocol contracts.


```solidity
function _registerTransferAuthorization(address op, address authority) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`authority`|`address`|Authority contract to authorize.|


