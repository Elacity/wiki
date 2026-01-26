## IIPMapperProvider

### IPAlreadyBound

```solidity
error IPAlreadyBound(bytes16 contentId)
```

_Thrown when an IP is already bound to a channel and tokenId_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contentId | bytes16 | The content ID that is already bound |

### IPBound

```solidity
event IPBound(bytes16 contentId, address channel, uint256 tokenId)
```

_Emitted when an IP is bound to a channel and tokenId_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contentId | bytes16 | The content ID that is bound |
| channel | address | The channel that the IP is bound to |
| tokenId | uint256 | The token ID that the IP is bound to |

### IPTracking

_Struct to track the IP tracking_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct IPTracking {
  address channel;
  uint256 tokenId;
}
```

### bindIP

```solidity
function bindIP(bytes16 _contentId, address channel, uint256 tokenId) external
```

_Bind an IP to a channel and tokenId_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _contentId | bytes16 | The content ID to bind |
| channel | address | The channel to bind the IP to |
| tokenId | uint256 | The token ID to bind the IP to |

### ipReference

```solidity
function ipReference(bytes16 _contentId) external view returns (address channel, uint256 tokenId)
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

