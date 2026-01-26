## SecurityModule

### UnsupprtedCipherSuite

```solidity
error UnsupprtedCipherSuite(bytes4 hash)
```

### CS_ECDHP256_RC4_ECDSAP256_KECCAK256

```solidity
bytes4 CS_ECDHP256_RC4_ECDSAP256_KECCAK256
```

### CS_ECDHP256_RC4_ECDSASECP256K1_KECCAK256

```solidity
bytes4 CS_ECDHP256_RC4_ECDSASECP256K1_KECCAK256
```

### __SecurityModule_init

```solidity
function __SecurityModule_init() internal
```

### registerHandle

```solidity
function registerHandle(bytes4 cs, address addr) external
```

### extractReq

```solidity
function extractReq(bytes payload) internal view returns (bytes4, address, bytes b)
```

