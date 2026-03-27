## ILicenseRequestHandler

### unwrapRequest

```solidity
function unwrapRequest(bytes payload) external view returns (bytes, bytes, bytes)
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
function wrapResponse(bytes pub, bytes res) external view returns (bytes)
```

Wrap a response into a raw response

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| pub | bytes |  |
| res | bytes | The response |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes | The raw response |

