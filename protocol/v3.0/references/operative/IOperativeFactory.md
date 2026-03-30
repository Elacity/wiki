# IOperativeFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/operative/IOperativeFactory.sol)

**Title:**
IOperativeFactory

Interface for Operative Factory contract


## Functions
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


### exists

check whether a contract have being created through the factory


```solidity
function exists(address opContract) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`opContract`|`address`|Address of the contract we want to check|


