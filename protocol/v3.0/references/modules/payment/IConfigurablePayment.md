# IConfigurablePayment
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/payment/IConfigurablePayment.sol)

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


