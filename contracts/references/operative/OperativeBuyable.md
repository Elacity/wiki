## OperativeBuyable

Buy once, play always.

For this contract type, `DISTRIBUTION_RIGHT` (3) token is issued only during mintings,
only creator have this right unless a new one is issued by contract owner

### OP_TYPE

```solidity
uint16 OP_TYPE
```

Connstant that represents the type of the digital asset.

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

Mint distribution right token to creator

_Called after initialization is complete_

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

