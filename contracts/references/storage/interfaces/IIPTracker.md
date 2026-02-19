## IIPTracker

Tracks which operative contract controls a given `(channel, tokenId)` pair.

### operator

```solidity
function operator(address channel, uint256 tokenId) external view returns (address)
```

Returns the operative address controlling an asset.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| channel | address | Channel/ledger contract address. |
| tokenId | uint256 | Asset token id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Operative contract address. |

