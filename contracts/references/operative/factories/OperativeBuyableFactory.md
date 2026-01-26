## OperativeBuyableFactory

### ContractCreated

```solidity
event ContractCreated(address creator, address op)
```

### dataStorage

```solidity
contract IStorage dataStorage
```

### exists

```solidity
mapping(address => bool) exists
```

_check whether a contract have being created through the factory_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

### constructor

```solidity
constructor(contract IStorage _dataStorage, contract IPaymentProcessorFactory _ppf, address _implementation) public
```

Factory constructors

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

### createOperativeContract

```solidity
function createOperativeContract(address creator, bytes16 contentId, string baseURI, address[] to, uint256[] ids, uint256[] amounts) internal returns (address)
```

### updateDataStorage

```solidity
function updateDataStorage(address _dataStorage) external
```

