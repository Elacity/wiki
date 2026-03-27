## IPMapperModule

Abstract module providing content ID to IP (channel and token ID) mapping capabilities.

_Provides the core storage and mapping mechanism to bind a content ID to an operative channel and token._

### IPMapperModuleStorage

```solidity
struct IPMapperModuleStorage {
  mapping(bytes16 => struct IIPMapperProvider.IPTracking) ipBindings;
}
```

### ipReference

```solidity
function ipReference(bytes16 _contentId) public view returns (address channel, uint256 tokenId)
```

Retrieves the channel and token ID bound to a specific content ID.

_Useful for querying the tracking details of mapped IP._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contentId | bytes16 | The 16-byte content ID to get the IP tracking for. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| channel | address | The address of the channel that the IP is bound to. |
| tokenId | uint256 | The identifier of the token that the IP is bound to. |

