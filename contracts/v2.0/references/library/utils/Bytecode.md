## Bytecode

Deploys contracts directly from raw creation bytecode.

### deployBytecode

```solidity
function deployBytecode(bytes bytecode) public returns (address)
```

Deploys a contract using EVM `create`.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| bytecode | bytes | Contract creation bytecode. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | retval Address of the deployed contract, or zero on failure. |

