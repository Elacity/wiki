## OperativeFactorySelectable

Abstract contract defining basic operative types and factory selection mechanism.

_Inheriting contracts must implement `_getOperativeFactory` to route to correct factories._

### OP_TYPE_FREE

```solidity
uint16 OP_TYPE_FREE
```

Type ID representing a Free Operative

### OP_TYPE_BUY

```solidity
uint16 OP_TYPE_BUY
```

Type ID representing a Buy Operative

### OP_TYPE_BUYSELL

```solidity
uint16 OP_TYPE_BUYSELL
```

Type ID representing a Buy&Sell Operative

### OP_TYPE_PPV

```solidity
uint16 OP_TYPE_PPV
```

Type ID representing a Pay-Per-View (PPV) Operative

### OP_TYPE_RENT

```solidity
uint16 OP_TYPE_RENT
```

Type ID representing a Rent Operative

### _getOperativeFactory

```solidity
function _getOperativeFactory(uint16 _opType) internal view virtual returns (address)
```

Retrieves the address of the factory corresponding to the given operative type.

_Must be implemented by child contracts to provide registry or router-based lookups._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _opType | uint16 | The type of the operative factory. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | The address of the operative factory. |

