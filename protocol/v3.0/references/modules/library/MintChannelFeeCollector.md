# MintChannelFeeCollector
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/library/MintFeeCollector.sol)

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

