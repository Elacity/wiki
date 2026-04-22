# IFeesInformation
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/storage/IFeesInformation.sol)

**Title:**
IFeesInformation

Exposes protocol fees for channel and media creation operations.


## Functions
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


## Structs
### FeeRecord
Fee configuration payload.


```solidity
struct FeeRecord {
    address recipient;
    uint256 weiValue;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address`|Fee recipient address (`address(0)` if disabled).|
|`weiValue`|`uint256`|Fee amount in wei (`0` if disabled).|

