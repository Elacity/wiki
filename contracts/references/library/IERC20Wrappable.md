## IERC20Wrappable

ERC-20 interface extension for wrapped native-token contracts.

### deposit

```solidity
function deposit() external payable
```

Wraps native currency into ERC-20 balance.

_`msg.value` defines wrapped amount._

### withdraw

```solidity
function withdraw(uint256 wad) external
```

Unwraps wrapped balance back into native currency.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| wad | uint256 | Amount to unwrap. |

