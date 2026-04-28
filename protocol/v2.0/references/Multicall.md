## OptimizedMulticall

Gas-efficient batching contract that executes multiple external calls in a single transaction.

Supports both state-changing calls (via `aggregate`) and read-only calls (via `aggregateStatic`).
Each call can carry its own ETH value and independently require success or allow graceful failure.

_Key properties:
- Built-in reentrancy guard on `aggregate` to prevent re-entrant batching.
- Per-call `requireSuccess` flag controls whether a single failure reverts the batch.
- The contract can hold ETH to fulfill value-bearing calls._

### CallFailed

```solidity
error CallFailed(uint256 index, string reason)
```

A call in the batch failed and its `requireSuccess` flag was `true`.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| index | uint256 | Zero-based index of the failed call within the batch |
| reason | string | Decoded revert reason (empty if the target reverted silently) |

### InvalidTarget

```solidity
error InvalidTarget()
```

The target address for a call is the zero address.

### InsufficientValue

```solidity
error InsufficientValue(uint256 value)
```

`msg.value` is less than the sum of all `Call.value` fields in the batch.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| value | uint256 | The `msg.value` that was provided |

### ReentrancyGuard

```solidity
error ReentrancyGuard()
```

A re-entrant call to `aggregate` was detected.

### Call

Describes a single call within a batch.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct Call {
  address target;
  uint256 value;
  bytes data;
  bool requireSuccess;
}
```

### Result

Outcome of a single call within a batch.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

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

Executes an array of calls in a single transaction, forwarding ETH as specified.

_Protected against reentrancy. Validates that `msg.value` covers the total value
required across all calls before execution begins._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| calls | struct OptimizedMulticall.Call[] | Ordered array of calls to execute |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| results | struct OptimizedMulticall.Result[] | Array of per-call results in the same order as `calls` |

### aggregateStatic

```solidity
function aggregateStatic(struct OptimizedMulticall.Call[] calls) external view returns (struct OptimizedMulticall.Result[] results)
```

Executes an array of read-only (`staticcall`) calls in a single transaction.

_No reentrancy guard is needed because `staticcall` cannot modify state.
The `value` field in each `Call` is ignored._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| calls | struct OptimizedMulticall.Call[] | Ordered array of calls to execute |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| results | struct OptimizedMulticall.Result[] | Array of per-call results in the same order as `calls` |

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

