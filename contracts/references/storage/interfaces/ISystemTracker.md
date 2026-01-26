## ISystemTracker

### ack

```solidity
function ack(address contractAddress) external
```

Recognize or whitelist a contract as a system contract

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contractAddress | address | Address of the contract to add |

### unAck

```solidity
function unAck(address contractAddress) external
```

Remove a contract from the ecosystem

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contractAddress | address | Address of the target contract |

### acknowledged

```solidity
function acknowledged(address contractAddress) external view returns (bool)
```

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| contractAddress | address | Contract address to check |

