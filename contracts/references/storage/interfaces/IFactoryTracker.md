## IFactoryTracker

Tracks operative factory contracts by operative type id.

### factories

```solidity
function factories(uint16 opType) external view returns (address)
```

Returns the factory address for an operative type.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| opType | uint16 | Operative type identifier. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Factory address mapped to `opType`. |

