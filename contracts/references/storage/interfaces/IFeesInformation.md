## IFeesInformation

_Interface for managing fee information in the Elacity DRM system.
This interface defines the structure and methods for handling fees related to
channel and media creation within the platform.

The fee system allows for configurable fees to be charged for creating channels
and media content, with fees being sent to designated recipient addresses._

### FeesState

_Structure representing the state of fees for a particular operation._

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

_Returns the current fee configuration for channel creation._

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | FeesState memory containing the recipient address and fee amount in wei         for creating new channels in the system. |
| [1] | address |  |

### mediaCreationFee

```solidity
function mediaCreationFee() external view returns (uint256, address)
```

_Returns the current fee configuration for media creation._

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | FeesState memory containing the recipient address and fee amount in wei         for creating new media content in the system. |
| [1] | address |  |

