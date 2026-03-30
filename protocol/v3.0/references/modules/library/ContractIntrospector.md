# ContractIntrospector
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/library/ContractIntrospector.sol)

**Title:**
ContractIntrospector

Reusable guards for storage-whitelisted callers.


## Functions
### whitelistOnly

Restricts access to acknowledged callers.


```solidity
modifier whitelistOnly(ISystemTracker store) ;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`store`|`ISystemTracker`|Storage contract used to verify acknowledgement.|


### whitelistRoleOnly

Restricts access to callers that either have required roles or are acknowledged.

Strict role-only guard used for capability-scoped write paths.


```solidity
modifier whitelistRoleOnly(ISystemTracker store, uint256 requiredRoleMask) ;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`store`|`ISystemTracker`|Storage contract used to verify roles and acknowledgement.|
|`requiredRoleMask`|`uint256`|Role bitmask required for strict-role path.|


### _whitelistOnly

Internal acknowledgment-only guard.


```solidity
function _whitelistOnly(ISystemTracker store) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`store`|`ISystemTracker`|Storage contract used to verify acknowledgement.|


### _whitelistRoleOnly

Internal role-aware strict guard.

Access is granted only when caller has `requiredRoleMask`.


```solidity
function _whitelistRoleOnly(ISystemTracker store, uint256 requiredRoleMask) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`store`|`ISystemTracker`|Storage contract used to verify roles and acknowledgement.|
|`requiredRoleMask`|`uint256`|Required capability role bitmap.|


## Errors
### UnrecognizedContractError
Thrown when the caller is not acknowledged by protocol storage.


```solidity
error UnrecognizedContractError(address caller);
```

### MissingContractRoleError
Thrown when caller is neither acknowledged nor assigned required role bits.


```solidity
error MissingContractRoleError(address caller, uint256 requiredRoleMask);
```

