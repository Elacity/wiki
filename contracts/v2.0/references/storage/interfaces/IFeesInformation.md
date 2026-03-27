## IFeesInformation

Exposes protocol fees for channel and media creation operations.

### FeesState

Fee configuration payload.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct FeesState {
  address recipient;
  uint256 weiValue;
}
```

### channelCreationFee

```solidity
function channelCreationFee() external view returns (uint256, address)
```

Returns current channel-creation fee configuration.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Fee amount in wei and recipient address. |
| [1] | address |  |

### mediaCreationFee

```solidity
function mediaCreationFee() external view returns (uint256, address)
```

Returns current media-creation fee configuration.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Fee amount in wei and recipient address. |
| [1] | address |  |

