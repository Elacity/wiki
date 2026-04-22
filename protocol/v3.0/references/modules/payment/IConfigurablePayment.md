# IConfigurablePayment
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/payment/IConfigurablePayment.sol)

**Title:**
IConfigurablePayment

Allows governance/admin to set the active payment processor.


## Functions
### setPaymentProcessor

Sets the payment processor contract.


```solidity
function setPaymentProcessor(address _payProc) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_payProc`|`address`|Processor contract address.|


