## SmartAccountUtils

_Library providing utilities for smart account compatibility
This handles the difference between EOAs and smart accounts when determining the sender_

### getSender

```solidity
function getSender() internal view returns (address sender)
```

_Get the appropriate sender address based on caller type_

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| sender | address | The sender address (msg.sender for smart accounts, origin for EOAs) |

### _checkContractOwner

```solidity
function _checkContractOwner(address ownedContract) internal view returns (bool, address)
```

### isAnyContract

```solidity
function isAnyContract(address account) internal view returns (bool)
```

_Check if an address is a contract_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The address to check |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bool | True if the address is a contract, false if it's an EOA |

