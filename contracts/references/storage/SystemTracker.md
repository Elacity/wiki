## SystemTracker

### acknowledged

```solidity
mapping(address => bool) acknowledged
```

Determine whether a contract is apart of the ecosystem

_Contract Addr -> aknownledged_

### ack

```solidity
function ack(address ca) external
```

Add new contract to be recognized as apart of the ecosystem

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ca | address | Address of the contract to add |

### unAck

```solidity
function unAck(address ca) external
```

Remove a contract from the ecosystem

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ca | address | Address of the contract to remove |

