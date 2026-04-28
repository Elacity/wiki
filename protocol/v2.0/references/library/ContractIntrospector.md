## ContractIntrospector

Reusable guards for storage-whitelisted callers.

### UnrecognizedContractError

```solidity
error UnrecognizedContractError(address caller)
```

Thrown when the caller is not acknowledged by protocol storage.

### whitelistOnly

```solidity
modifier whitelistOnly(contract ISystemTracker store)
```

Restricts access to acknowledged callers.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract ISystemTracker | Storage contract used to verify acknowledgement. |

