## OperativeCommon

### ACCESS_TOKEN

```solidity
uint256 ACCESS_TOKEN
```

_Constant that represents the access token id._

### ROYALTY_SHARE

```solidity
uint256 ROYALTY_SHARE
```

_Constant that represents the royalty share token id._

### DISTRIBUTION_RIGHT

```solidity
uint256 DISTRIBUTION_RIGHT
```

This token type aims to control the ability to sell
access token, only owner of it can sell and trade access tokens

### contentId

```solidity
bytes16 contentId
```

Represents the identifier of the digital asset over the ecosystem
its value complies with RFC-4122 speicifcation and is 128-bits long as a requirements of
[https://dashif-documents.azurewebsites.net/Guidelines-Security](https://dashif-documents.azurewebsites.net/Guidelines-Security/master/Guidelines-Security.html#content-key)

_see also [https://www.ietf.org/rfc/rfc4122.txt](https://www.ietf.org/rfc/rfc4122.txt)_

### dataStorage

```solidity
contract IStorage dataStorage
```

### royaltyHolders

```solidity
struct EnumerableSet.AddressSet royaltyHolders
```

### accessTransferAuthorized

```solidity
modifier accessTransferAuthorized(address from, uint256[] ids)
```

### constructor

```solidity
constructor() internal
```

### initialize

```solidity
function initialize(contract IStorage _dataStorage, bytes16 _contentId, string baseURI) public virtual
```

### _accessTransferAuthorized

```solidity
function _accessTransferAuthorized(address from, uint256[] ids) internal view returns (bool)
```

### mintBatchEveryone

```solidity
function mintBatchEveryone(address[] to, uint256[] ids, uint256[] amounts, bytes data) public
```

### _checkAccessFor

```solidity
function _checkAccessFor(address account, uint256[] ids) internal view returns (struct IOperative.AccessLevel[])
```

### checkAccess

```solidity
function checkAccess(address account) external view virtual returns (struct IOperative.AccessLevel[])
```

_Returns the access level of a given account to the digital asset._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The address of the account to check. |

### uri

```solidity
function uri(uint256 id) public view returns (string)
```

### royaltyInfo

```solidity
function royaltyInfo(uint256 _salePrice) external view returns (struct IERC2981Enhanced.RoyaltyInfo[])
```

### _update

```solidity
function _update(address from, address to, uint256[] ids, uint256[] values) internal virtual
```

_See {ERC1155-_update}._

### _remapRoyaltyHoldings

```solidity
function _remapRoyaltyHoldings(address from, address to, uint256 amount) internal
```

### hasTradeAccess

```solidity
function hasTradeAccess(address account, uint256 tkId) public view returns (bool)
```

### metadataURI

```solidity
function metadataURI() external view returns (string)
```

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```

### receive

```solidity
receive() external payable virtual
```

-------------------------------

