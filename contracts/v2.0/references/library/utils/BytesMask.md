## BytesMask

Produces fixed-width masks used to partition token-id ranges by plan id.

### maskU16

```solidity
function maskU16(uint8 id) internal pure returns (bytes16 r)
```

Builds a `bytes16` mask containing a sentinel byte and the provided id.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| id | uint8 | Identifier inserted into the mask. Must be in `[1, 255]`. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| r | bytes16 | Generated mask value. |

