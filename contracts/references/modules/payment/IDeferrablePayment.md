# IDeferrablePayment
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/payment/IDeferrablePayment.sol)

**Inherits:**
IERC165

**Title:**
IDeferrablePayment

This interface defines a payment processor that defer
payment statements by wrapping it with `defer` and `commit`
executions


## Functions
### defer

Opens a deferred payment session for a payer.


```solidity
function defer(address from) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address whose payments will be deferred.|


### commit

Commits and settles deferred payments for a payer.


```solidity
function commit(address from, address _payToken) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address whose deferred queue is committed.|
|`_payToken`|`address`|Payment token used for settlement.|


### isDeferred

Returns whether an address currently has an active deferred session.


```solidity
function isDeferred(address from) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if deferred mode is active.|


## Events
### PaymentCommitted
Emitted when a deferred payment is finalized.


```solidity
event PaymentCommitted(
    address indexed from, address indexed recipient, address indexed paymentToken, uint256 amount
);
```

## Errors
### OutOfCommitError
Thrown when `commit` is called without a matching deferred session.


```solidity
error OutOfCommitError(address sender);
```

### EmptyCommitError
Thrown when a deferred session has no pending payments to commit.


```solidity
error EmptyCommitError();
```

### UnauthorizedCommitCaller
Thrown when commit is called by a contract different from the session opener.


```solidity
error UnauthorizedCommitCaller(address from, address caller, address expected);
```

### DeferredSessionOwnerMismatch
Thrown when another contract tries to take over an active deferred session.


```solidity
error DeferredSessionOwnerMismatch(address from, address currentOwner, address attemptedOwner);
```

