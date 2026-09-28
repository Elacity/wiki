# BytesMask
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/library/BytesMask.sol)

**Title:**
BytesMask

Produces fixed-width masks used to partition token-id ranges by plan id.


## Functions
### maskU16

Builds a `bytes16` mask containing a sentinel byte and the provided id.


```solidity
function maskU16(uint8 id) internal pure returns (bytes16 r);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`id`|`uint8`|Identifier inserted into the mask. Must be in `[1, 255]`.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`r`|`bytes16`|Generated mask value.|


