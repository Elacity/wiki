## ILicenseProvider

Defines registration and retrieval flows for encrypted license material.

### IPRegistered

```solidity
event IPRegistered(address ledger, bytes16 contentId, bytes32 ipHash)
```

Emitted when content metadata is registered in a target ledger.

### License

License payload returned to a consumer.

```solidity
struct License {
  address issuer;
  address toAddress;
  struct IPEntity entity;
  bytes[] key;
}
```

### registerIPWithKey

```solidity
function registerIPWithKey(address ledger, bytes16 _contentId, bytes _ipKey) external returns (bytes32, bytes)
```

Registers content key material into a ledger and returns its hash.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | Target contract maintaining key references. |
| _contentId | bytes16 | Content identifier. |
| _ipKey | bytes | Encoded encryption key payload. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes32 | Hash of the stored key payload and raw response bytes. |
| [1] | bytes |  |

### acquireLicense

```solidity
function acquireLicense(bytes payload) external view returns (bytes4, bytes)
```

Resolves a license request payload into an encoded license response.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| payload | bytes | Cipher-suite-prefixed request payload. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes4 | Cipher-suite id and encoded response payload. |
| [1] | bytes |  |

