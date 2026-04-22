# ISubscribable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/subscription/ISubscribable.sol)

**Title:**
ISubscribable

Minimal subscription primitives for contracts that sell access plans.


## Functions
### subscribePlan

Subscribes `msg.sender` to a plan.


```solidity
function subscribePlan(uint8 planId, bytes calldata args) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Plan identifier.|
|`args`|`bytes`|ABI-encoded subscription args for extensible workflow data.|


### unsubscribePlan

Cancels the caller's subscription to the given plan.


```solidity
function unsubscribePlan(uint8 planId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`planId`|`uint8`|Plan identifier.|


### hasActiveSubscription

Returns whether an account currently has an active subscription.


```solidity
function hasActiveSubscription(address subscriber) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`subscriber`|`address`|Account to evaluate.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if the account has a non-expired subscription.|


