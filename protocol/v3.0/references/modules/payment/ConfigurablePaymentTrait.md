# ConfigurablePaymentTrait
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/payment/ConfigurablePaymentTrait.sol)

**Title:**
ConfigurablePaymentTrait

Exposes an extendable interface to configure a customized payment processor.

Allows inheriting contracts to integrate with different `IPaymentProcessor` implementations.


## State Variables
### CONFIGURABLE_PAYMENT_TRAIT_STORAGE_LOCATION

```solidity
bytes32 private constant CONFIGURABLE_PAYMENT_TRAIT_STORAGE_LOCATION =
    0x9891e261a5c8be6063227e7a8e46fc4e9fb912f364bed89fd056e3c5ce595000
```


## Functions
### _getConfigurablePaymentTraitStorage


```solidity
function _getConfigurablePaymentTraitStorage() private pure returns (ConfigurablePaymentTraitStorage storage $);
```

### paymentProcessor

Address of the implementation of the payment processor


```solidity
function paymentProcessor() public view virtual returns (IPaymentProcessor);
```

### _setPaymentProcessor

Sets the payment processor.


```solidity
function _setPaymentProcessor(address _payProc) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_payProc`|`address`|The address of the payment processor.|


### setPaymentProcessor

Sets the payment processor.


```solidity
function setPaymentProcessor(address _payProc) external virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_payProc`|`address`|The address of the payment processor.|


### _afterProcessorSet

Called after the payment processor is set.


```solidity
function _afterProcessorSet(address _payProc) internal virtual;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_payProc`|`address`|The address of the payment processor.|


## Structs
### ConfigurablePaymentTraitStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.ConfigurablePaymentTrait


```solidity
struct ConfigurablePaymentTraitStorage {
    IPaymentProcessor paymentProcessor;
}
```

