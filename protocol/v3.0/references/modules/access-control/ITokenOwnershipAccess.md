# ITokenOwnershipAccess
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/access-control/ITokenOwnershipAccess.sol)

`ITokenOwnershipAccess` interface defines the way to manage access based on
token (ERC20 or ERC721) ownership.
To enable flexibility, all contracts that supports `balanceOf` function are intended to
be supported.


## Functions
### ownershipThreshold

Constant that hold threshold for each token


```solidity
function ownershipThreshold(address tokenAddress) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenAddress`|`address`|Address of the token|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Value of the threshold in `uint256`|


### configureTokenOwnershipAccess

Configure token-based access.


```solidity
function configureTokenOwnershipAccess(TokenAccessThreshold[] memory _input) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_input`|`TokenAccessThreshold[]`|dataset to apply to the contract|


### checkTokenOwnershipAccess

Check whether an account have access based on its ownership of registered tokens


```solidity
function checkTokenOwnershipAccess(address account) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address to check access for|


## Structs
### TokenAccessThreshold
This struct represents a set of settings that define the Token-based access


```solidity
struct TokenAccessThreshold {
    address tokenAddress;
    uint256 threshold;
}
```

**Properties**

|Name|Type|Description|
|----|----|-----------|
|`tokenAddress`|`address`|Address of the token to consider (ERC20, ERC721)|
|`threshold`|`uint256`|Amount minimal to enable the access|

