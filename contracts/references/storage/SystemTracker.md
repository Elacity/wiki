## SystemTracker

### UnauthorizedAckError

```solidity
error UnauthorizedAckError(address caller)
```

### acknowledged

```solidity
mapping(address => bool) acknowledged
```

Determine whether a contract is apart of the ecosystem

_Contract Addr -> aknownledged_

### ack

```solidity
function ack(address ca) external virtual
```

Add new contract to be recognized as apart of the ecosystem

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ca | address | Address of the contract to add |

### unAck

```solidity
function unAck(address ca) external virtual
```

Remove a contract from the ecosystem

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ca | address | Address of the contract to remove |

