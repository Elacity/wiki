## TokenURIConversion

Converts [ERC-1155 metadata URIs](https://eips.ethereum.org/EIPS/eip-1155#metadata) containing `{id}` into concrete token URIs.

_Replaces `{id}` with a 64-char lowercase hex token id (without `0x`) per ERC-1155._

### convert

```solidity
function convert(string uri_, uint256 tokenId) public pure returns (string tokenURI)
```

Substitutes `{id}` in a metadata URI with the token id representation.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| uri_ | string | URI template, potentially containing `{id}`. |
| tokenId | uint256 | Token identifier to inject. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenURI | string | Converted URI string. |

