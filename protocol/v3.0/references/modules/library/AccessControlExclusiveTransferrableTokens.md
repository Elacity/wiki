# AccessControlExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/library/AccessControlExclusiveTransferrableTokens.sol)

**Inherits:**
[ExclusiveTransferrableTokens](/contracts/modules/library/ExclusiveTransferrableTokens.md), AccessControlUpgradeable

**Title:**
AccessControlExclusiveTransferrableTokens

Abstract contract that implements the `ExclusiveTransferrableTokens` interface
and extends the `AccessControlUpgradeable` contract.


## Functions
### __AccessControlExclusiveTransferrableTokens_init


```solidity
function __AccessControlExclusiveTransferrableTokens_init() internal onlyInitializing;
```

### allowTransferOf


```solidity
function allowTransferOf(address operator, uint256 tkId) public override onlyRole(DEFAULT_ADMIN_ROLE);
```

