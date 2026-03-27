# ITradableErrors
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/trade/ITradableErrors.sol)

**Title:**
ITradableErrors

Provides error definitions for tradable flow.


## Errors
### ZeroValueError
Indicates a zero value error


```solidity
error ZeroValueError();
```

### NotAllowedError
Indicates the caller is not allowed to perform the action


```solidity
error NotAllowedError(address from);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address that called the function|

### NotApprovedError
Indicates the caller is not approved to perform the action


```solidity
error NotApprovedError(address _operative);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_operative`|`address`|The address of the operative|

### InvalidOperativeError
Indicates the operative is invalid


```solidity
error InvalidOperativeError(address _operative);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_operative`|`address`|The address of the operative|

### InvalidPaymentTokenError
Indicates the payment token is invalid


```solidity
error InvalidPaymentTokenError(address _payToken);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_payToken`|`address`|The address of the payment token|

### InsufficientOwningError
Indicates the owning quantity is insufficient


```solidity
error InsufficientOwningError(address _operative, address _owner, uint256 _balance, uint256 _qt);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_operative`|`address`|The address of the operative|
|`_owner`|`address`|The address of the owner|
|`_balance`|`uint256`|The balance of the owner|
|`_qt`|`uint256`|The quantity requested|

### PriceFulfillmentError
Indicates the price fulfillment is invalid


```solidity
error PriceFulfillmentError(uint256 value, uint256 toPay);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`value`|`uint256`|The value of the trade|
|`toPay`|`uint256`|The amount to pay|

### AvailabilityError
Indicates the withdrawal quantity is higher than listed


```solidity
error AvailabilityError(uint256 actual, uint256 requested);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`actual`|`uint256`|Amount of tokens stated|
|`requested`|`uint256`|Amount of tokens requested|

### NoOverrideError
prevents override of a trade terms


```solidity
error NoOverrideError(address from);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address that called the function|

