## SecurityModule

Maintains cipher-suite handlers used to process encrypted license payloads.

_Handlers are registered by the security manager role and resolved from the
first 4 bytes of incoming payloads._

### UnsupprtedCipherSuite

```solidity
error UnsupprtedCipherSuite(bytes4 hash)
```

Thrown when a cipher-suite hash has no registered handler.

### CS_ECDHP256_RC4_ECDSAP256_KECCAK256

```solidity
bytes4 CS_ECDHP256_RC4_ECDSAP256_KECCAK256
```

Identifier for `ECDH-P256_RC4_ECDSA-P256-KECCAK256`.

### CS_ECDHP256_RC4_ECDSASECP256K1_KECCAK256

```solidity
bytes4 CS_ECDHP256_RC4_ECDSASECP256K1_KECCAK256
```

Identifier for `ECDH-P256_RC4_ECDSA-SECP256K1-KECCAK256`.

### registerHandle

```solidity
function registerHandle(bytes4 cs, address addr) external
```

Registers or updates a handler for a cipher suite.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| cs | bytes4 | 4-byte cipher-suite identifier. |
| addr | address | Handler contract address for the cipher suite. |

### extractReq

```solidity
function extractReq(bytes payload) internal view returns (bytes4, address, bytes b)
```

Parses a license payload into cipher suite and handler-specific bytes.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| payload | bytes | ABI-encoded payload prefixed with a 4-byte cipher-suite id. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes4 | Cipher-suite id, registered handler address, and decoded payload body. |
| [1] | address |  |
| b | bytes |  |

