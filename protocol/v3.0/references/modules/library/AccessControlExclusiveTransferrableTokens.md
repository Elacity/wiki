# AccessControlExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/library/AccessControlExclusiveTransferrableTokens.sol)

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

