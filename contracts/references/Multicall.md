## OptimizedMulticall

_A gas-efficient and secure implementation of multicall functionality
that allows batching multiple calls in a single transaction.

Key Features:
- Optimized gas usage with minimal overhead
- Support for both regular and static calls
- Built-in reentrancy protection
- Detailed error reporting
- Value forwarding support_

### CallFailed

```solidity
error CallFailed(uint256 index, string reason)
```

### InvalidTarget

```solidity
error InvalidTarget()
```

### InsufficientValue

```solidity
error InsufficientValue(uint256 value)
```

### ReentrancyGuard

```solidity
error ReentrancyGuard()
```

### Call

_Call data structure with value support_

```solidity
struct Call {
  address target;
  uint256 value;
  bytes data;
  bool requireSuccess;
}
```

### Result

_Result structure for detailed call outcomes_

```solidity
struct Result {
  bool success;
  bytes returnData;
}
```

### preventReentrancy

```solidity
modifier preventReentrancy()
```

### constructor

```solidity
constructor() public
```

### aggregate

```solidity
function aggregate(struct OptimizedMulticall.Call[] calls) external payable returns (struct OptimizedMulticall.Result[] results)
```

_Executes multiple calls in a single transaction_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| calls | struct OptimizedMulticall.Call[] | Array of calls to execute |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| results | struct OptimizedMulticall.Result[] | Array of results from each call |

### aggregateStatic

```solidity
function aggregateStatic(struct OptimizedMulticall.Call[] calls) external view returns (struct OptimizedMulticall.Result[] results)
```

_Executes multiple static (view/pure) calls in a single transaction_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| calls | struct OptimizedMulticall.Call[] | Array of static calls to execute |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| results | struct OptimizedMulticall.Result[] | Array of results from each call |

### _getRevertMsg

```solidity
function _getRevertMsg(bytes _returnData) internal pure returns (string)
```

_Extracts revert message from return data_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _returnData | bytes | Return data from failed call |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | Revert message string |

### receive

```solidity
receive() external payable
```

_Allows contract to receive ETH_

