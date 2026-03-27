## FactoryTracker

Keeps the operative factory address associated with each operative type id.

### InvalidAddress

```solidity
error InvalidAddress(address needle)
```

Thrown when a zero-address factory is provided.

### factories

```solidity
mapping(uint16 => address) factories
```

Operative type id => factory contract address.

### setOperativeFactory

```solidity
function setOperativeFactory(uint16 opType, address factoryAddr) external
```

Sets the factory address for a specific operative type.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| opType | uint16 | Operative type id. |
| factoryAddr | address | Factory contract address. |

