## StorageModule

Shared module that stores the protocol `IStorage` reference for inheriting contracts.

_Acknowledgement must be granted explicitly by trusted authorities._

### _store

```solidity
contract IStorage _store
```

Address of the active `IStorage` implementation.

### setStorage

```solidity
function setStorage(address s) internal
```

Sets the storage contract reference.

_We need to be very careful here, this method is under security monitoring, it could change/deprecate anytime_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| s | address | Address expected to implement `IStorage`. |

