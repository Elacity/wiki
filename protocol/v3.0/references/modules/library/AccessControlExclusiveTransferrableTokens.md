# AccessControlExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/library/AccessControlExclusiveTransferrableTokens.sol)

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

