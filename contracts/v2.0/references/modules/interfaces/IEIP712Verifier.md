## IEIP712Verifier

Verifies EIP-712 license request signatures for the protocol.

### verifyLicenseRequest

```solidity
function verifyLicenseRequest(struct LicenseRequest lr, bytes sig) external view returns (address)
```

Verifies the signer of a license request payload.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| lr | struct LicenseRequest | License request payload. |
| sig | bytes | EIP-712 signature bytes. |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | address | Address recovered from signature verification. |

### protocolVersion

```solidity
function protocolVersion() external view returns (string)
```

Returns the protocol version used for domain separation and compatibility.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | Current protocol version string. |

