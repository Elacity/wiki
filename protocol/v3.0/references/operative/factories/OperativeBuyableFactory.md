# OperativeBuyableFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/operative/factories/OperativeBuyableFactory.sol)

**Inherits:**
[BeaconUpgradeableFactory](../../modules/proxy/BeaconUpgradeableFactory.md), [IOperativeFactory](../../operative/IOperativeFactory.md)

**Title:**
OperativeBuyableFactory

Factory that deploys `OperativeBuyable` (type 1) beacon proxies.
Each call to `createFromBytes` produces a fully-initialised operative with
minted tokens, distribution rights, a payment processor, and ownership
transferred to the content creator.

Inherits `BeaconUpgradeableFactory` so every proxy shares the same
upgradeable implementation contract.


## State Variables
### PAYMENT_PROCESSOR_FACTORY
Factory used to create per-operative payment processors.


```solidity
IPaymentProcessorFactory private immutable PAYMENT_PROCESSOR_FACTORY
```


### cstore
Shared ecosystem storage contract.


```solidity
IStorage public cstore
```


### exists
Tracks every proxy address deployed by this factory.


```solidity
mapping(address => bool) public exists
```


## Functions
### constructor

Deploys the factory, registering the beacon implementation and ecosystem dependencies.

**Note:**
docs-ignore: true


```solidity
constructor(IStorage _cstore, IPaymentProcessorFactory _ppf, address _implementation)
    BeaconUpgradeableFactory(_implementation, msg.sender);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_cstore`|`IStorage`|        Shared ecosystem storage contract.|
|`_ppf`|`IPaymentProcessorFactory`|           Factory that creates payment-processor instances.|
|`_implementation`|`address`|Address of the `OperativeBuyable` logic contract behind the beacon.|


### createFromBytes

create the new Operative contract in charge of handling all Operative Tokens
related to a given Digital Asset. Generally, created contract is a ERC1155


```solidity
function createFromBytes(address creator, bytes memory data) external returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address of the creator of the contract|
|`data`|`bytes`|Data to be used to initialize the contract, we use bytes to provide more flexibiity when creating the Operative contract itself|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the created contract|


### _createOperativeContract

Creates a new `OperativeBuyable` proxy.


```solidity
function _createOperativeContract(
    address creator,
    bytes16 contentId,
    string memory baseURI,
    address[] memory to,
    uint256[] memory ids,
    uint256[] memory amounts
) internal returns (address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address that will own the new operative.|
|`contentId`|`bytes16`|Content ID of the operative.|
|`baseURI`|`string`|Base URI for the operative.|
|`to`|`address[]`|Array of addresses to mint tokens to.|
|`ids`|`uint256[]`|Array of token IDs to mint.|
|`amounts`|`uint256[]`|Array of amounts to mint.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address`|Address of the newly-deployed proxy.|


### updateDataStorage

Replaces the ecosystem storage reference. Owner-only.


```solidity
function updateDataStorage(address _cstore) external onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_cstore`|`address`|New `IStorage` contract address.|


## Events
### ContractCreated
Emitted when a new `OperativeBuyable` proxy is deployed.


```solidity
event ContractCreated(address indexed creator, address indexed op);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`creator`|`address`|Address that will own the new operative.|
|`op`|`address`|     Address of the newly-deployed proxy.|

