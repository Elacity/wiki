# MintFeeCollectorBase
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/library/MintFeeCollector.sol)

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

