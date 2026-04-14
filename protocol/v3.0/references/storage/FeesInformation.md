# FeesInformation
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/storage/FeesInformation.sol)

**Inherits:**
[IFeesInformation](/contracts/storage/IFeesInformation.md), OwnableUpgradeable

**Title:**
FeesInformation

Abstract contract implementing fee management for the Elacity DRM system.
This contract provides a secure, upgradeable implementation for managing fees
associated with channel and media creation operations. It uses ERC-7201 storage
patterns to ensure storage collision safety in upgradeable contracts.
Key features:
- Owner-only fee configuration
- Separate fee structures for channels and media
- Automatic validation of fee recipient addresses
- Zero-fee support (when weiValue is 0, recipient is automatically set to address(0))

This contract is abstract and must be inherited by implementation contracts.
It follows the ERC-7201 standard for namespaced storage to prevent storage collisions
in proxy-based upgradeable contracts.


## State Variables
### FEES_INFORMATION_STORAGE_LOCATION
Storage slot for IFeesInformationStorage, calculated using ERC-7201 standard.
Formula: keccak256(abi.encode(uint256(keccak256("elacity.drm.storage.IFeesInformation")) - 1)) & ~bytes32(uint256(0xff))


```solidity
bytes32 private constant FEES_INFORMATION_STORAGE_LOCATION =
    0xf48e423161368c2fa06d35b85545e6d487a763bf9e2406891ad5ce9844e16f00
```


## Functions
### _getIFeesInformationStorage


```solidity
function _getIFeesInformationStorage() private pure returns (IFeesInformationStorage storage $);
```

### channelCreationFee

Returns current channel-creation fee configuration.


```solidity
function channelCreationFee() external view returns (uint256, address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Fee amount in wei and recipient address.|
|`<none>`|`address`||


### mediaCreationFee

Returns current media-creation fee configuration.


```solidity
function mediaCreationFee() external view returns (uint256, address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Fee amount in wei and recipient address.|
|`<none>`|`address`||


### protocolShares

Returns protocol royalty-share mint configuration.

`weiValue` is royalty share units out of 1000 (not wei). Example: 50 = 5%.


```solidity
function protocolShares() external view returns (uint256, address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Share units reserved for protocol royalty minting.|
|`<none>`|`address`|Recipient address for protocol royalty shares.|


### assignChannelCreationFee

Sets the fee configuration for channel creation operations.


```solidity
function assignChannelCreationFee(uint256 weiValue, address feeRecipient) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`weiValue`|`uint256`|The fee amount in wei. If 0, no fee is charged and recipient is set to address(0).|
|`feeRecipient`|`address`|The address that will receive fee payments. Must be valid if weiValue > 0.|


### assignMediaCreationFee

Sets the fee configuration for media creation operations.


```solidity
function assignMediaCreationFee(uint256 weiValue, address feeRecipient) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`weiValue`|`uint256`|The fee amount in wei. If 0, no fee is charged and recipient is set to address(0).|
|`feeRecipient`|`address`|The address that will receive fee payments. Must be valid if weiValue > 0.|


### assignProtocolShares

Sets protocol royalty-share mint configuration.

`amount` is in share units out of 1000 (not wei). If zero, recipient is cleared.


```solidity
function assignProtocolShares(uint256 amount, address feeRecipient) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|Share units to mint for protocol royalty.|
|`feeRecipient`|`address`|Recipient of protocol royalty shares.|


## Errors
### InvalidValue
Error thrown when an invalid fee value is provided.
This occurs when a non-zero fee amount is specified with a zero recipient address.


```solidity
error InvalidValue(uint256 value);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`value`|`uint256`|The invalid fee value that was provided|

### ProtocolSharesExceedMax
Thrown when protocol share amount exceeds the allowed maximum.


```solidity
error ProtocolSharesExceedMax(uint256 amount, uint256 max);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`amount`|`uint256`|The invalid protocol share amount.|
|`max`|`uint256`|The maximum allowed protocol shares.|

## Structs
### IFeesInformationStorage
Storage structure for fees information using ERC-7201 namespaced storage.

**Note:**
storage-location: erc7201:elacity.drm.storage.IFeesInformation


```solidity
struct IFeesInformationStorage {
    FeeRecord channel;
    FeeRecord media;
    FeeRecord protocolShares;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`channel`|`FeeRecord`|Fee configuration for channel creation operations.|
|`media`|`FeeRecord`|Fee configuration for media creation operations.|
|`protocolShares`|`FeeRecord`|Protocol royalty share configuration (weiValue = share units, not wei).|

