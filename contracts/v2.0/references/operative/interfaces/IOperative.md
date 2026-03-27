## IOperative

### AccessLevel

```solidity
struct AccessLevel {
  bool haveAccess;
  uint256 entitlement;
}
```

### contentId

```solidity
function contentId() external view returns (bytes16)
```

Represents the identifier of the digital asset over the ecosystem
its value complies with RFC-4122 speicifcation and is 128-bits long as a requirements of
[https://dashif-documents.azurewebsites.net/Guidelines-Security](https://dashif-documents.azurewebsites.net/Guidelines-Security/master/Guidelines-Security.html#content-key)

_see also [https://www.ietf.org/rfc/rfc4122.txt](https://www.ietf.org/rfc/rfc4122.txt)_

### OP_TYPE

```solidity
function OP_TYPE() external view returns (uint16)
```

Connstant that represents the type of the digital asset.

### ACCESS_TOKEN

```solidity
function ACCESS_TOKEN() external view returns (uint256)
```

_Constant that represents the access token id._

### ROYALTY_SHARE

```solidity
function ROYALTY_SHARE() external view returns (uint256)
```

_Constant that represents the royalty share token id._

### DISTRIBUTION_RIGHT

```solidity
function DISTRIBUTION_RIGHT() external view returns (uint256)
```

_Constant that represents the distribution right token id._

### checkAccess

```solidity
function checkAccess(address account) external view returns (struct IOperative.AccessLevel[])
```

_Returns the access level of a given account to the digital asset._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The address of the account to check. |

