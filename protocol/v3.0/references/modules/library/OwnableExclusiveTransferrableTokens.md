# OwnableExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/library/OwnableExclusiveTransferrableTokens.sol)

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

