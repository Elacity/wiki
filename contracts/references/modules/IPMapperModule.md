## IPMapperModule

### IPMapperModuleStorage

```solidity
struct IPMapperModuleStorage {
  mapping(bytes16 => struct IIPMapperProvider.IPTracking) ipBindings;
}
```

### bindIP

```solidity
function bindIP(bytes16 _contentId, address channel, uint256 tokenId) external
```

_Map the contentId to the operative in charge of the IP_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contentId | bytes16 | The content ID to map |
| channel | address | The channel that the IP is bound to |
| tokenId | uint256 | The token ID that the IP is bound to |

### ipReference

```solidity
function ipReference(bytes16 _contentId) public view returns (address channel, uint256 tokenId)
```

_Get the IP tracking for a content ID_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contentId | bytes16 | The content ID to get the IP tracking for |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| channel | address | The channel that the IP is bound to |
| tokenId | uint256 | The token ID that the IP is bound to |

