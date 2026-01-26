## FactoryTracker

### InvalidAddress

```solidity
error InvalidAddress(address needle)
```

The address provided is invalid

### factories

```solidity
mapping(uint16 => address) factories
```

Factory registry data
Operative Type -> Factory address

### __FactoryTracker_init

```solidity
function __FactoryTracker_init() internal
```

_Internal function to initialize the contract
This is called by the root contract (CoreStorage) during its initialization_

### setOperativeFactory

```solidity
function setOperativeFactory(uint16 opType, address factoryAddr) external
```

Set factory address for a given type of operative flow

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| opType | uint16 | uint16 - Type of the operative flow |
| factoryAddr | address | address - Address of the factory contract |

