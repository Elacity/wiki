## ECDHP256_RC4_ECDSAP256_KECCAK256

### constructor

```solidity
constructor() public
```

### unwrapRequest

```solidity
function unwrapRequest(bytes payload) external pure returns (bytes, bytes, bytes)
```

Unwrap a raw request into a request that can be processed by the handler

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| payload | bytes | The raw request |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes | sig The signature of the request |
| [1] | bytes | meta The meta data of the request |
| [2] | bytes | The request |

### wrapResponse

```solidity
function wrapResponse(bytes remotePubKey, bytes res) external view returns (bytes)
```

