# OwnableExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/library/OwnableExclusiveTransferrableTokens.sol)

**Inherits:**
[ExclusiveTransferrableTokens](/contracts/modules/library/ExclusiveTransferrableTokens.md), OwnableUpgradeable

**Title:**
OwnableExclusiveTransferrableTokens

Abstract contract that implements the `ExclusiveTransferrableTokens` interface
and extends the `OwnableUpgradeable` contract.


## Functions
### __OwnableExclusiveTransferrableTokens_init


```solidity
function __OwnableExclusiveTransferrableTokens_init() internal onlyInitializing;
```

### allowTransferOf


```solidity
function allowTransferOf(address operator, uint256 tkId) public override;
```

### _checkOwnerLater

Check ownership for transfer authorization.

Virtual to allow overrides with context-aware checks (e.g. acknowledged contracts).


```solidity
function _checkOwnerLater() internal view virtual;
```

