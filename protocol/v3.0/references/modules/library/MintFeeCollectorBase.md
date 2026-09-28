# MintFeeCollectorBase
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/library/MintFeeCollector.sol)

**Inherits:**
[StorageModule](/contracts/modules/core/StorageModule.md)

**Title:**
MintFeeCollector

Collects protocol fees on digital-asset (media) minting.


## Functions
### _collectConfiguredFee


```solidity
function _collectConfiguredFee(uint256 fee, address recipient)
    internal
    returns (uint256 excess, bool refundedToPayer);
```

### _refundOrForwardExcess


```solidity
function _refundOrForwardExcess(address payer, address recipient, uint256 amount) internal returns (bool);
```

