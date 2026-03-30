# IOperative
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/operative/IOperative.sol)

**Inherits:**
IERC1155, [IERC2981Enhanced](/contracts/modules/royalty/IERC2981Enhanced.md)

**Title:**
IOperative

Interface for Operative contract


## Functions
### contentId

Represents the identifier of the digital asset over the ecosystem
its value complies with RFC-4122 speicifcation and is 128-bits long as a requirements of
[https://dashif-documents.azurewebsites.net/Guidelines-Security](https://dashif-documents.azurewebsites.net/Guidelines-Security/master/Guidelines-Security.html#content-key)

see also [https://www.ietf.org/rfc/rfc4122.txt](https://www.ietf.org/rfc/rfc4122.txt)


```solidity
function contentId() external view returns (bytes16);
```

### OP_TYPE

Connstant that represents the type of the digital asset.


```solidity
function OP_TYPE() external view returns (uint16);
```

### tokenURI

Returns the resolved metadata URI for a token.


```solidity
function tokenURI(uint256 tokenId) external view returns (string memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`tokenId`|`uint256`|Token id to query.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|URI string for the token metadata.|


### checkAccess

Returns the access level of a given account to the digital asset.


```solidity
function checkAccess(address account) external view returns (AccessLevel[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|The address of the account to check.|


## Structs
### AccessLevel
Struct to represent access level


```solidity
struct AccessLevel {
    bool haveAccess;
    uint256 entitlement;
}
```

