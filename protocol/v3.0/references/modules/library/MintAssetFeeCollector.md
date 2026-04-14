# MintAssetFeeCollector
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/library/MintFeeCollector.sol)

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

