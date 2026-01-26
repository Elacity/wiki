## IEIP712Verifier

`IEIP712Verifier` provides the minimal functionnalities of a EIP-712
verifier in sense of Elacity ecosystem

### verifyLicenseRequest

```solidity
function verifyLicenseRequest(struct LicenseRequest lr, bytes sig) external view returns (address)
```

Process verification of a license request

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| lr | struct LicenseRequest | License request payload |
| sig | bytes | Request signature |

### protocolVersion

```solidity
function protocolVersion() external view returns (string)
```

Display the protocol verison. This method is relevant only since version
`0.5.0` of the smart contracts.

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | string | `string` The protocol version |

