# OwnableExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/library/OwnableExclusiveTransferrableTokens.sol)

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

