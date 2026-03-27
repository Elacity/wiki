# MintChannelFeeCollector
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/library/MintFeeCollector.sol)

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

