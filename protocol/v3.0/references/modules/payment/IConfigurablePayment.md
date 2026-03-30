# IConfigurablePayment
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/payment/IConfigurablePayment.sol)

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


