# MintChannelFeeCollector
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/library/MintFeeCollector.sol)

**Inherits:**
[MintFeeCollectorBase](/contracts/modules/library/MintFeeCollectorBase.md)

Collects protocol fees on channel creation.


## Functions
### collectChannelCreationFee


```solidity
modifier collectChannelCreationFee() ;
```

### _collectChannelCreationFee


```solidity
function _collectChannelCreationFee() internal;
```

## Events
### ExcessChannelCreationPaymentHandled

```solidity
event ExcessChannelCreationPaymentHandled(address indexed payer, uint256 amount, bool refundedToPayer);
```

## Errors
### InsufficientChannelFee

```solidity
error InsufficientChannelFee(uint256 required, uint256 sent);
```

