# PaymentProcessorFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/payment/PaymentProcessorFactory.sol)

**Inherits:**
[IPaymentProcessorFactory](/contracts/modules/payment/IPaymentProcessorFactory.md), [BeaconUpgradeableFactory](/contracts/modules/proxy/BeaconUpgradeableFactory.md)

**Title:**
PaymentProcessorFactory

Deploys beacon-proxied `WithdrawablePaymentProcessor` instances.


## Functions
### constructor

**Note:**
docs-ignore: true


```solidity
constructor(address _implementation) BeaconUpgradeableFactory(_implementation, msg.sender);
```

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


## Events
### PaymentProcessorCreated
Emitted when a new payment processor proxy is created.


```solidity
event PaymentProcessorCreated(
    address indexed recipient, bool indexed shouldTransferFund, address indexed paymentProcessor
);
```

