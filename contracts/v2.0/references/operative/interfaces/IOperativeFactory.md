## IOperativeFactory

### createFromBytes

```solidity
function createFromBytes(address creator, bytes data) external returns (address)
```

_create the new Operative contract in charge of handling all Operative Tokens
related to a given Digital Asset. Generally, created contract is a ERC1155_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| creator | address | Address of the creator of the contract |
| data | bytes | Data to be used to initialize the contract, we use bytes to provide more flexibiity when creating the Operative contract itself |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of the created contract |

### exists

```solidity
function exists(address opContract) external view returns (bool)
```

_check whether a contract have being created through the factory_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| opContract | address | Address of the contract we want to check |

