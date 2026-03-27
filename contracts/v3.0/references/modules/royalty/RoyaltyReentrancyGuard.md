# RoyaltyReentrancyGuard
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/royalty/RoyaltyReentrancyGuard.sol)

**Title:**
RoyaltyReentrancyGuard

Transient-storage reentrancy guard for royalty payout flows.

Default optimized variant. For networks without EIP-1153 support, swap inheritance/import
to `RoyaltyReentrancyGuardNonTranscient`.


## State Variables
### ROYALTY_REENTRANCY_GUARD_TRANSIENT_SLOT
Transient slot used by the royalty payout reentrancy guard.
Deterministic and isolated to avoid accidental overlap with other guard slots.


```solidity
bytes32 private constant ROYALTY_REENTRANCY_GUARD_TRANSIENT_SLOT =
    keccak256("elacity.drm.transient.RoyaltyReentrancyGuard.v1")
```


## Functions
### nonReentrant

Prevents nested reentrant entry into guarded flows.


```solidity
modifier nonReentrant() ;
```

### _nonReentrantBefore


```solidity
function _nonReentrantBefore() internal;
```

### _nonReentrantAfter


```solidity
function _nonReentrantAfter() internal;
```

### _reentrancyGuardEntered


```solidity
function _reentrancyGuardEntered() internal view returns (bool entered);
```

## Errors
### ReentrancyGuardReentrantCall
Matches OpenZeppelin's error surface for compatibility.


```solidity
error ReentrancyGuardReentrantCall();
```

