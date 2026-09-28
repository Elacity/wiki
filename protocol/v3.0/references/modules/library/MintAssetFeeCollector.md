# MintAssetFeeCollector
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/library/MintFeeCollector.sol)

**Inherits:**
[MintFeeCollectorBase](/contracts/modules/library/MintFeeCollectorBase.md)


## Functions
### collectAssetMintFee


```solidity
modifier collectAssetMintFee() ;
```

### _collectAssetMintFee


```solidity
function _collectAssetMintFee() internal;
```

## Events
### ExcessAssetMintPaymentHandled

```solidity
event ExcessAssetMintPaymentHandled(address indexed payer, uint256 amount, bool refundedToPayer);
```

## Errors
### InsufficientMintFee

```solidity
error InsufficientMintFee(uint256 required, uint256 sent);
```

