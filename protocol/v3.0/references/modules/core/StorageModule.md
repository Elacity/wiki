# StorageModule
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/core/StorageModule.sol)

**Title:**
StorageModule

Shared module that stores the protocol `IStorage` reference for inheriting contracts.

Acknowledgement must be granted explicitly by trusted authorities.


## State Variables
### cstore
Address of the active `IStorage` implementation.


```solidity
IStorage public cstore
```


## Functions
### setStorage

Sets the storage contract reference.

We need to be very careful here, this method is under security monitoring, it could change/deprecate anytime


```solidity
function setStorage(address s) internal;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`s`|`address`|Address expected to implement `IStorage`.|


