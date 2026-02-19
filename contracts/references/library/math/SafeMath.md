## SafeMath

Thin wrappers around OpenZeppelin `Math.try*` helpers with revert-on-failure behavior.

### mul

```solidity
function mul(uint256 a, uint256 b) internal pure returns (uint256)
```

Multiplies two numbers and reverts on overflow.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| a | uint256 | Left operand. |
| b | uint256 | Right operand. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Product of `a` and `b`. |

### div

```solidity
function div(uint256 a, uint256 b) internal pure returns (uint256)
```

Divides two numbers and reverts when divisor is zero.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| a | uint256 | Dividend. |
| b | uint256 | Divisor. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Quotient of `a / b`. |

### sub

```solidity
function sub(uint256 a, uint256 b) internal pure returns (uint256)
```

Subtracts two numbers and reverts on underflow.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| a | uint256 | Left operand. |
| b | uint256 | Right operand. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Difference of `a - b`. |

### add

```solidity
function add(uint256 a, uint256 b) internal pure returns (uint256)
```

Adds two numbers and reverts on overflow.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| a | uint256 | Left operand. |
| b | uint256 | Right operand. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Sum of `a + b`. |

### mod

```solidity
function mod(uint256 a, uint256 b) internal pure returns (uint256)
```

Computes modulo and reverts when divisor is zero.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| a | uint256 | Left operand. |
| b | uint256 | Right operand. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Remainder of `a % b`. |

