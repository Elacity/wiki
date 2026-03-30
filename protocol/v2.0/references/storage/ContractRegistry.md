## ContractRegistry

Stores and exposes ecosystem contract addresses by deterministic slot keys.

### contractAt

```solidity
mapping(bytes32 => address) contractAt
```

Mapping from slot identifier to contract address.

### _registerAt

```solidity
function _registerAt(bytes32 slot, address value) internal
```

Registers a contract address at a slot.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| slot | bytes32 | Slot key used to store the contract address. |
| value | address | Contract address to register. |

