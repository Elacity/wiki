# PaymentModule
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/payment/PaymentModule.sol)

**Inherits:**
ContextUpgradeable

**Title:**
PaymentModule

Abstract module dealing with payment routing, deferment, and platform fee deductions.

Core payment logic to be inherited by modules requiring value transfers and splits.


## Functions
### _deferPaymentIfQualifiedBefore


```solidity
function _deferPaymentIfQualifiedBefore(
    IPaymentProcessor payer,
    address from,
    IERC2981Enhanced.RoyaltyInfo[] memory rs
) internal returns (bool deferred, IDeferrablePayment committer, uint256 totalAmount);
```

### _deferPaymentIfQualifiedAfter


```solidity
function _deferPaymentIfQualifiedAfter(
    IPaymentProcessor payer,
    address from,
    address _payToken,
    bool deferred,
    IDeferrablePayment committer,
    uint256 totalAmount
) internal;
```

### __PaymentModule_init


```solidity
function __PaymentModule_init() internal onlyInitializing;
```

### receive


```solidity
receive() external payable virtual;
```

### _payAmount

main entrypoint of the payment processing


```solidity
function _payAmount(
    IPaymentProcessor paymentProcessor,
    address from,
    address to,
    uint256 _amount,
    address _payToken
) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`paymentProcessor`|`IPaymentProcessor`||
|`from`|`address`||
|`to`|`address`|recipient of the payment|
|`_amount`|`uint256`|the amount in `wei` to pay|
|`_payToken`|`address`|address of the payment method|


### _deductPlatformFee


```solidity
function _deductPlatformFee(IStorage store, address from, uint256 _amount, address _payToken)
    internal
    returns (uint256 amount);
```

## Events
### PaymentLog
Event triggered on each payment processed regardless of how it is processed


```solidity
event PaymentLog(address indexed from, address indexed to, uint256 amount, address indexed paymentToken);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|sender of the payment|
|`to`|`address`|recipient of the payment|
|`amount`|`uint256`|the amount in `wei` to pay|
|`paymentToken`|`address`|address of the payment method|

