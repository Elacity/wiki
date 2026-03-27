# TokenIdGenerator
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/library/TokenIdGenerator.sol)

**Title:**
TokenIdGenerator

Generates pseudo-random token identifiers from user-provided seeds.


Basic Usage**
```solidity
contract TokenRegistry {
using TokenIdGenerator for string;
error OutOfBound(bytes32 value);
function mintNFT(string memory tokenURI) external {
bytes32 _tokenId = _tokenURI.derivateId();
if (_tokenId <= tokenSpaceProhibited) {
revert OutOfBound(_tokenId);
}
// ... mint statement here with the predefined `tokenId`
}
}
```


## Functions
### derivateId

Generates a pseudo-random bytes32 ID


```solidity
function derivateId(bytes32 seed) internal view returns (bytes32 result);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seed`|`bytes32`|A user-provided seed for added randomness|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`result`|`bytes32`|A pseudo-random bytes32|


### derivateId

Generates a pseudo-random bytes32 ID using a string as a seed


```solidity
function derivateId(string memory seed) internal view returns (bytes32);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seed`|`string`|A user-provided string seed for added randomness|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bytes32`|A pseudo-random bytes32 ID|


