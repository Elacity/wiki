# WithdrawReentrancyGuardNonTranscient
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/payment/WithdrawReentrancyGuard.sol)

**Title:**
WithdrawReentrancyGuardNonTranscient

Fallback non-transient withdraw reentrancy guard using ERC-7201 namespaced storage.

Use this variant on networks without EIP-1153 support.


## State Variables
### WITHDRAW_REENTRANCY_GUARD_STORAGE_LOCATION
Storage slot for _withdrawLocked, calculated using ERC-7201 standard.
This ensures the storage location is unique and prevents collisions with other contracts.
Formula: keccak256(abi.encode(uint256(keccak256("elacity.drm.storage.IWithdrawReentrancyGuard")) - 1)) & ~bytes32(uint256(0xff))

**Note:**
storage-location: erc7201:elacity.drm.storage.IWithdrawReentrancyGuard


```solidity
bytes32 private constant WITHDRAW_REENTRANCY_GUARD_STORAGE_LOCATION =
    0x77a6dd6698bc555cf0492f1fa2266c87488f41449fe4a4f9ecaf37402678dd00
```


## Functions
### noReentrantWithdraw

Modifier to prevent reentrancy in withdrawRewards.


```solidity
modifier noReentrantWithdraw() ;
```

### _noReentrantWithdraw


```solidity
function _noReentrantWithdraw() internal;
```

### _getIWithdrawReentrancyGuardStorage

Internal function to access the namespaced storage for WithdrawReentrancyGuard.
Uses inline assembly to directly access the storage slot defined by ERC-7201.


```solidity
function _getIWithdrawReentrancyGuardStorage() internal pure returns (WithdrawReentrancyGuardStorage storage s);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`s`|`WithdrawReentrancyGuardStorage`|Storage reference to the WithdrawReentrancyGuardStorage struct|


### _isWithdrawLocked

Internal function to check if the withdrawRewards function is locked.


```solidity
function _isWithdrawLocked() internal view returns (bool);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|bool True if the withdrawRewards function is locked, false otherwise|


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

## Structs
### WithdrawReentrancyGuardStorage
Storage structure for WithdrawReentrancyGuard using ERC-7201 namespaced storage.
This prevents storage collisions in upgradeable proxy contracts.

**Note:**
storage-location: erc7201:elacity.drm.storage.IWithdrawReentrancyGuard


```solidity
struct WithdrawReentrancyGuardStorage {
    bool _withdrawLocked;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`_withdrawLocked`|`bool`|Reentrancy lock for withdrawRewards|

