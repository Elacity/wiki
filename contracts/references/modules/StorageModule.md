## StorageModule

Shared module that stores the protocol `IStorage` reference for inheriting contracts.

_`setStorage` also acknowledges the caller contract in storage._

### _store

```solidity
contract IStorage _store
```

Address of the active `IStorage` implementation.

### setStorage

```solidity
function setStorage(address s) internal
```

Sets the storage contract reference and acknowledges `address(this)`.

_We need to be very careful here, this method is under security monitoring, it could change/deprecate anytime_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| s | address | Address expected to implement `IStorage`. |

