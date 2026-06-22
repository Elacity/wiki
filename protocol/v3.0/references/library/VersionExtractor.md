# VersionExtractor
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/library/VersionExtractor.sol)

**Title:**
VersionExtractor

Simple library to extract version string from version hex value


## Functions
### toString

Extracts the version string from the version hex value


```solidity
function toString(uint64 versionHex) internal pure returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`versionHex`|`uint64`|The hex value used in the reinitializer modifier|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|The version string (e.g., "2.1")|


