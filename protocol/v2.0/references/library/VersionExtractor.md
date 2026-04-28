## VersionExtractor

Simple library to extract version string from version hex value

### toString

```solidity
function toString(uint64 versionHex) internal pure returns (string)
```

Extracts the version string from the version hex value

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| versionHex | uint64 | The hex value used in the reinitializer modifier |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | The version string (e.g., "2.1") |

