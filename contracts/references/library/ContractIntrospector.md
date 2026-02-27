## ContractIntrospector

Reusable guards for contract-only and storage-whitelisted callers.

### NotContractError

```solidity
error NotContractError(address caller)
```

Thrown when the caller is not a deployed contract.

### UnrecognizedContractError

```solidity
error UnrecognizedContractError(address caller)
```

Thrown when the caller contract is not acknowledged by protocol storage.

### whitelistOnly

```solidity
modifier whitelistOnly(contract ISystemTracker store)
```

Restricts access to acknowledged contract callers.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract ISystemTracker | Storage contract used to verify acknowledgement. |

### isContract

```solidity
modifier isContract()
```

Restricts access to deployed contract callers.

### _isContract

```solidity
function _isContract(address account) internal view returns (bool)
```

Returns whether an address currently has runtime bytecode.

_This method is under development and could change or be deprecated at any moment for [security reason](https://www.cyfrin.io/glossary/bypass-contract-size-check-hack-solidity-code-example)_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address to check. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | `true` when code size is greater than zero. |

