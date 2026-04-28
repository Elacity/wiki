## IContractRegistry

Maps deterministic storage slots to protocol contract addresses.

### contractAt

```solidity
function contractAt(bytes32 slot) external view returns (address)
```

Returns the contract address registered at a slot.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| slot | bytes32 | Storage slot key to query. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Contract address stored at `slot`. |

