## IIPMapperProvider

Maps external content identifiers to on-chain `(channel, tokenId)` references.

### IPAlreadyBound

```solidity
error IPAlreadyBound(bytes16 contentId)
```

Thrown when attempting to bind an already-mapped content id.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contentId | bytes16 | Content id that already has a mapping. |

### IPBound

```solidity
event IPBound(bytes16 contentId, address channel, uint256 tokenId)
```

Emitted when a content id is mapped to a token reference.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contentId | bytes16 | Bound content id. |
| channel | address | Channel contract address. |
| tokenId | uint256 | Token id inside the channel. |

### IPTracking

Mapping value describing where a content id is represented on-chain.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct IPTracking {
  address channel;
  uint256 tokenId;
}
```

### ipReference

```solidity
function ipReference(bytes16 _contentId) external view returns (address channel, uint256 tokenId)
```

Returns the token reference mapped to a content id.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contentId | bytes16 | Content id to resolve. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| channel | address | Channel contract address. |
| tokenId | uint256 | Token id inside the channel. |

