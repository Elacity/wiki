# MintAssetFeeCollector
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/library/MintFeeCollector.sol)

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

