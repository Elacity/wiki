# ConstraintedTokenId
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/library/ConstraintedTokenId.sol)

**Title:**
ConstraintedTokenId

Produces fixed-width masks used to partition token-id ranges by plan id.


## State Variables
### PROHIBITED_SPACE
Upper bound of the prohibited token-id space.
Generated token IDs are mapped to strictly greater values.


```solidity
bytes32 private constant PROHIBITED_SPACE = 0x00000000000000000000000000000000ffffffffffffffffffffffffffffffff
```


## Functions
### generateTokenId

Generates a constrained token id from a token URI seed.


```solidity
function generateTokenId(string memory _tokenUri) external view returns (uint256);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_tokenUri`|`string`|Seed string used to derive the token id.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Token id guaranteed to be strictly greater than `PROHIBITED_SPACE`.|


