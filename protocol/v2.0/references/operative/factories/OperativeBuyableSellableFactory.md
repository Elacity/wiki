## OperativeBuyableSellableFactory

Factory that deploys `OperativeBuyableSellable` (type 2) beacon proxies.
Identical to `OperativeBuyableFactory` but additionally configures the `resellerCut`
on each new operative, enabling secondary-market resale.

_The ABI-encoded `data` passed to `createFromBytes` includes an extra `uint16 resellerCut`
field compared to the type-1 factory._

### ContractCreated

```solidity
event ContractCreated(address creator, address op)
```

Emitted when a new `OperativeBuyableSellable` proxy is deployed.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| creator | address | Address that will own the new operative. |
| op | address | Address of the newly-deployed proxy. |

### dataStorage

```solidity
contract IStorage dataStorage
```

Shared ecosystem storage contract.

### exists

```solidity
mapping(address => bool) exists
```

Tracks every proxy address deployed by this factory.

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
function createOperativeContract(address creator, bytes16 contentId, string baseURI, address[] to, uint256[] ids, uint256[] amounts, uint16 resellerCut) internal returns (address)
```

_Creates a new `OperativeBuyableSellable` proxy._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| creator | address | Address that will own the new operative. |
| contentId | bytes16 | Content ID of the operative. |
| baseURI | string | Base URI for the operative. |
| to | address[] | Array of addresses to mint tokens to. |
| ids | uint256[] | Array of token IDs to mint. |
| amounts | uint256[] | Array of amounts to mint. |
| resellerCut | uint16 | Reseller cut percentage. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address of the newly-deployed proxy. |

### updateDataStorage

```solidity
function updateDataStorage(address _dataStorage) external
```

Replaces the ecosystem storage reference. Owner-only.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| _dataStorage | address | New `IStorage` contract address. |

