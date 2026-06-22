# AccessControlExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/library/AccessControlExclusiveTransferrableTokens.sol)

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

