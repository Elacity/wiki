## MintFeeCollectorErrors

Shared custom errors for mint fee collectors.

### InsufficientFee

```solidity
error InsufficientFee()
```

Thrown when the supplied payment is below configured fee.

### InvalidRecipient

```solidity
error InvalidRecipient()
```

Thrown when a non-zero fee is configured with a zero recipient.

## MintChannelFeeCollector

Modifier helper that charges configured channel creation fees.

### collectFeeWith

```solidity
modifier collectFeeWith(address s, uint256 amount)
```

Charges channel creation fee before continuing execution.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| s | address | Storage contract exposing fee settings. |
| amount | uint256 | Payment amount provided by caller. |

## MintAssetFeeCollector

Modifier helper that charges configured media/asset minting fees.

### collectFeeWith

```solidity
modifier collectFeeWith(address s, uint256 amount)
```

Charges media creation fee before continuing execution.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| s | address | Storage contract exposing fee settings. |
| amount | uint256 | Payment amount provided by caller. |

