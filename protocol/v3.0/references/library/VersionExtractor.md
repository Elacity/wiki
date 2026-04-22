# VersionExtractor
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/library/VersionExtractor.sol)

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


