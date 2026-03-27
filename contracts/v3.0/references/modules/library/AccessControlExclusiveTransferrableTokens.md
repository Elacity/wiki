# AccessControlExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/library/AccessControlExclusiveTransferrableTokens.sol)

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

