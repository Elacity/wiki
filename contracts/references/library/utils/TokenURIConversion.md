## TokenURIConversion

This library take in charge of converting ECR-1155 tokenURI in format as defined at
[https://eips.ethereum.org/EIPS/eip-1155#metadata](https://eips.ethereum.org/EIPS/eip-1155#metadata) to a human
readable URI

### convert

```solidity
function convert(string uri_, uint256 tokenId) public pure returns (string tokenURI)
```

Convert the ERC1155 metadata URI to ERC721 metata URI by subsituting the `{id}`
with the hex encoded tokenId.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| uri_ | string | The mask of the tokenURI |
| tokenId | uint256 | The ID of the token |

