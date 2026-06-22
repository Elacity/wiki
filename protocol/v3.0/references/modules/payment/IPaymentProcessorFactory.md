# IPaymentProcessorFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/payment/IPaymentProcessorFactory.sol)

**Title:**
IPaymentProcessorFactory

Creates payment processor instances bound to a recipient.


## Functions
### createPaymentProcessor

Deploys a new payment processor.


```solidity
function createPaymentProcessor(address recipient, bool _shouldTransferFund) external returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`recipient`|`address`|Default reward recipient.|
|`_shouldTransferFund`|`bool`|Whether processor should perform direct transfer.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the deployed processor.|


