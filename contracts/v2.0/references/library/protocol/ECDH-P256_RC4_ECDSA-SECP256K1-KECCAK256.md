## ECDHP256_RC4_ECDSASECP256K1_KECCAK256

License handler using P-256 ECDH, RC4 encryption, and secp256k1 ECDSA signatures.

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
| remotePubKey | bytes | ABI-encoded requester ECDH public key `(x, y)`. |
| res | bytes | Plain response payload before encryption. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes | ABI-encoded `(dhPubX, dhPubY, sigPubX, sigPubY, sig, cipher)` packet. |

