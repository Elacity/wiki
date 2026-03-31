# MintFeeCollectorBase
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/library/MintFeeCollector.sol)

**Inherits:**
[StorageModule](../../modules/core/StorageModule.md)

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

