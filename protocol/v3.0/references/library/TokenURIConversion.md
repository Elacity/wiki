# TokenURIConversion
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/library/TokenURIConversion.sol)

**Title:**
TokenURIConversion

Converts [ERC-1155 metadata URIs](https://eips.ethereum.org/EIPS/eip-1155#metadata) containing `{id}` into concrete token URIs.

Replaces `{id}` with a 64-char lowercase hex token id (without `0x`) per ERC-1155.


## Functions
### convert

Substitutes `{id}` in a metadata URI with the token id representation.


```solidity
function convert(string memory uri_, uint256 tokenId) public pure returns (string memory tokenURI);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`uri_`|`string`|URI template, potentially containing `{id}`.|
|`tokenId`|`uint256`|Token identifier to inject.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`tokenURI`|`string`|Converted URI string.|


