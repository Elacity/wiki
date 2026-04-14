# IConfigurablePayment
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/payment/IConfigurablePayment.sol)

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


