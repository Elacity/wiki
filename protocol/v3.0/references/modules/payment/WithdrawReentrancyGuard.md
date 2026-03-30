# WithdrawReentrancyGuard
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/payment/WithdrawReentrancyGuard.sol)

**Title:**
WithdrawReentrancyGuard

Prevents reentrancy in `withdrawRewards` using transient storage (EIP-1153).

Default optimized variant. For networks without EIP-1153 support, swap inheritance/import
to `WithdrawReentrancyGuardNonTranscient`.


## State Variables
### WITHDRAW_REENTRANCY_GUARD_TRANSIENT_SLOT
Transient slot used by the withdraw reentrancy guard.
Deterministic and isolated to avoid accidental overlap with other guard slots.


```solidity
bytes32 private constant WITHDRAW_REENTRANCY_GUARD_TRANSIENT_SLOT =
    keccak256("elacity.drm.transient.WithdrawReentrancyGuard.v1")
```


## Functions
### noReentrantWithdraw

Modifier to prevent reentrancy in withdrawRewards.


```solidity
modifier noReentrantWithdraw() ;
```

### _noReentrantWithdraw

Internal function to prevent reentrancy in withdrawRewards.


```solidity
function _noReentrantWithdraw() internal;
```

### _isWithdrawLocked

Internal function to check if the withdrawRewards function is locked.


```solidity
function _isWithdrawLocked() internal view returns (bool locked);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`locked`|`bool`|True if the withdrawRewards function is locked, false otherwise|


### _setWithdrawLocked

Internal function to set the withdrawRewards function lock.


```solidity
function _setWithdrawLocked(bool _withdrawLocked) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_withdrawLocked`|`bool`|True to lock the withdrawRewards function, false otherwise|


## Errors
### RewardsReentrantCall
Thrown when a reentrant withdrawRewards() call is detected.


```solidity
error RewardsReentrantCall();
```

