## StorageModule

Abstract contract that provide ability to hold a `IStorage` contract type in
`_store` as a public property. It also provides a `setStorage` internal method to allow
to set a value to that property.

### _store

```solidity
contract IStorage _store
```

Address of the `IStorage` implementation

### setStorage

```solidity
function setStorage(address s) internal
```

Set value of the storage contract

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| s | address | Address of the storage contract, it should be under `IStorage` interface. No checking will happen during this statement so it's important the address passed as argument is well under that wanted interface. |

