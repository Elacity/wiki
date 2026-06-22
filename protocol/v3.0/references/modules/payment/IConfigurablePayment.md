# IConfigurablePayment
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/payment/IConfigurablePayment.sol)

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


