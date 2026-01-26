## LicenseModule

### InvalidSignature

```solidity
error InvalidSignature(address accessor, address signer)
```

### WrongSignatureFormat

```solidity
error WrongSignatureFormat()
```

### AccessDenied

```solidity
error AccessDenied(address accessor, address channel, uint256 tokenId)
```

### LICENSE_MANAGER_ROLE

```solidity
bytes32 LICENSE_MANAGER_ROLE
```

### __LicenseModule_init

```solidity
function __LicenseModule_init(string contractName, string version) internal
```

### _issueLicenseFor

```solidity
function _issueLicenseFor(address accessor, struct IPEntity _entity) internal view returns (struct ILicenseProvider.License)
```

_Issue a license for a given Digital Asset
accessor the address of the accessor_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| accessor | address |  |
| _entity | struct IPEntity | The entity to issue the license for TODO: find a more secure way to store the license and never expose the private key instead we could implement a kind of signed key that can only be decrypt from a dApp |

### _requestUserAccess

```solidity
function _requestUserAccess(contract IStorage store, address accessor, address ledger, uint256 tokenId) internal view returns (bool haveAccess, uint256[] entitlements)
```

Check the access to a given Digital Asset, this will check over Operative contract
in order to detect if a user have token(s) that allows to access the asset

### _requestAccess

```solidity
function _requestAccess(contract IStorage store, address ledger, uint256 tokenId) internal view returns (bool haveAccess, uint256[] entitlements)
```

Check the access to a given Digital Asset, this will check over Operative contract
in order to detect is the sender have token(s) that allows to access the asset

### _acquireLicense

```solidity
function _acquireLicense(contract IStorage store, bytes payload) internal view returns (bytes4, bytes)
```

_Acquire a license for a given Digital Asset_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| store | contract IStorage | The storage contract |
| payload | bytes | The payload to be processed |

#### Return Values

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | bytes4 | cs The callback selector |
| [1] | bytes |  |

### acquireLicense

```solidity
function acquireLicense(bytes req) external view virtual returns (bytes4, bytes)
```

### registerIPWithKey

```solidity
function registerIPWithKey(address ledger, bytes16 _contentId, bytes _ipKey) external returns (bytes32, bytes)
```

_Register a new IP in the system using a given key
for version 1.0, this method is the only relevant as we deal with clearkey
public and private are used to encrypt and decrypt the content and client on playback
need to know the private key to decrypt the content in a clear manner_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| ledger | address | The address of the ledger contract |
| _contentId | bytes16 | The content id of the asset |
| _ipKey | bytes | The key to use to encrypt the content |

### _registerIPWithKey

```solidity
function _registerIPWithKey(address ledger, bytes16 _contentId, bytes _ipKey) internal returns (bytes32, bytes)
```

