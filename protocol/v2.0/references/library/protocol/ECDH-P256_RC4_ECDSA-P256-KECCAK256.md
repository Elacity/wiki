## ECDHP256_RC4_ECDSAP256_KECCAK256

License request handler using P-256 ECDH, RC4 payload encryption, and P-256 ECDSA signatures.

### constructor

```solidity
constructor() public
```

Initializes cryptographic primitives for this handler.

### unwrapRequest

```solidity
function unwrapRequest(bytes payload) external pure returns (bytes, bytes, bytes)
```

Unwraps request payload components expected by this cipher suite.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| payload | bytes | ABI-encoded `(sig, remoteX, remoteY, req)` tuple. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes | Signature, encoded remote public key, and encrypted request payload. |
| [1] | bytes |  |
| [2] | bytes |  |

### wrapResponse

```solidity
function wrapResponse(bytes remotePubKey, bytes res) external view returns (bytes)
```

Wraps and signs a license response for the requester.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| remotePubKey | bytes | ABI-encoded requester public key `(x, y)`. |
| res | bytes | Plain response payload before encryption. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes | ABI-encoded `(pubX, pubY, sig, cipher)` response packet. |

