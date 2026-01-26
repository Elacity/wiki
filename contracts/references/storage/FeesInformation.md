## FeesInformation

_Abstract contract implementing fee management for the Elacity DRM system.

This contract provides a secure, upgradeable implementation for managing fees
associated with channel and media creation operations. It uses ERC-7201 storage
patterns to ensure storage collision safety in upgradeable contracts.

Key features:
- Owner-only fee configuration
- Separate fee structures for channels and media
- Automatic validation of fee recipient addresses
- Zero-fee support (when weiValue is 0, recipient is automatically set to address(0))

This contract is abstract and must be inherited by implementation contracts.
It follows the ERC-7201 standard for namespaced storage to prevent storage collisions
in proxy-based upgradeable contracts._

### InvalidValue

```solidity
error InvalidValue(uint256 value)
```

_Error thrown when an invalid fee value is provided.
This occurs when a non-zero fee amount is specified with a zero recipient address._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| value | uint256 | The invalid fee value that was provided |

### IFeesInformationStorage

```solidity
struct IFeesInformationStorage {
  struct IFeesInformation.FeesState channel;
  struct IFeesInformation.FeesState media;
}
```

### __FeesInformation_init

```solidity
function __FeesInformation_init(address initialOwner) internal
```

_Internal function to initialize the contract
This is called by the root contract (CoreStorage) during its initialization_

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

### assignChannelCreationFee

```solidity
function assignChannelCreationFee(uint256 weiValue, address feeRecipient) external
```

When weiValue is 0, the feeRecipient is automatically set to address(0) regardless of the input.
Reverts with InvalidValue if weiValue > 0 and feeRecipient is address(0).

Requirements:
- Caller must be the contract owner
- If weiValue > 0, feeRecipient must not be address(0)

_Sets the fee configuration for channel creation operations.
Only the contract owner can call this function._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| weiValue | uint256 | The fee amount in wei to charge for channel creation.                 If set to 0, no fee will be charged and feeRecipient is automatically set to address(0). |
| feeRecipient | address | The address that will receive the fee payments.                     Must be a valid address if weiValue > 0. |

### assignMediaCreationFee

```solidity
function assignMediaCreationFee(uint256 weiValue, address feeRecipient) external
```

When weiValue is 0, the feeRecipient is automatically set to address(0) regardless of the input.
Reverts with InvalidValue if weiValue > 0 and feeRecipient is address(0).

Requirements:
- Caller must be the contract owner
- If weiValue > 0, feeRecipient must not be address(0)

_Sets the fee configuration for media creation operations.
Only the contract owner can call this function._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| weiValue | uint256 | The fee amount in wei to charge for media creation.                 If set to 0, no fee will be charged and feeRecipient is automatically set to address(0). |
| feeRecipient | address | The address that will receive the fee payments.                     Must be a valid address if weiValue > 0. |

