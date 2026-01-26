## OperativeBuyableSellable

Buy once, play always, resell

### UnauthorizedDistrbutorError

```solidity
error UnauthorizedDistrbutorError(address from)
```

Only holders of `DISTRIBUTION_RIGHT` tokens are authorized

### ResellerCutOverflow

```solidity
error ResellerCutOverflow(uint256 value)
```

The reseller cut should not exceed 100%

### OP_TYPE

```solidity
uint16 OP_TYPE
```

Connstant that represents the type of the digital asset.

### resellerCut

```solidity
uint16 resellerCut
```

### onlyDistributor

```solidity
modifier onlyDistributor(address from)
```

### constructor

```solidity
constructor() public
```

### initialize

```solidity
function initialize(contract IStorage _dataStorage, bytes16 _contentId, string baseURI) public virtual
```

### setupDistributionRights

```solidity
function setupDistributionRights(address creator) external
```

_Public function to setup initial distribution rights
This must be called after initialization in a separate transaction_

### _isDistributor

```solidity
function _isDistributor(address from) internal view returns (bool)
```

### setResellerCut

```solidity
function setResellerCut(uint16 _resellerCut) public
```

### checkAccess

```solidity
function checkAccess(address account) external view returns (struct IOperative.AccessLevel[])
```

Determine which kind of access an account have to the digital asset
We will make this check by level

### _update

```solidity
function _update(address from, address to, uint256[] ids, uint256[] values) internal virtual
```

_See {ERC1155-_update}._

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```

