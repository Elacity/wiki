## ContractIntrospector

### NotContractError

```solidity
error NotContractError(address caller)
```

### UnrecognizedContractError

```solidity
error UnrecognizedContractError(address caller)
```

### whitelistOnly

```solidity
modifier whitelistOnly(contract IStorage store)
```

### isContract

```solidity
modifier isContract()
```

### _isContract

```solidity
function _isContract(address account) internal view returns (bool)
```

