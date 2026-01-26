## OperativeFactorySelectable

### OP_TYPE_FREE

```solidity
uint16 OP_TYPE_FREE
```

### OP_TYPE_BUY

```solidity
uint16 OP_TYPE_BUY
```

### OP_TYPE_BUYSELL

```solidity
uint16 OP_TYPE_BUYSELL
```

### OP_TYPE_PPV

```solidity
uint16 OP_TYPE_PPV
```

### OP_TYPE_RENT

```solidity
uint16 OP_TYPE_RENT
```

### _getOperativeFactory

```solidity
function _getOperativeFactory(uint16 _opType) internal view virtual returns (address)
```

_Returns the address of the operative factory for the given type._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _opType | uint16 | The type of the operative factory. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | The address of the operative factory. |

