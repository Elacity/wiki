## IExclusiveTransferrableTokens

### allowedTransfer

```solidity
function allowedTransfer(address operator, uint256 tkId) external view returns (bool)
```

### allowTransferOf

```solidity
function allowTransferOf(address operator, uint256 tkId) external
```

## ExclusiveTransferrableTokens

### UnauthorizedTransfer

```solidity
error UnauthorizedTransfer()
```

### OperatorAllowed

```solidity
event OperatorAllowed(address initiator, address operator, uint256 tokenId)
```

### allowedTransfer

```solidity
function allowedTransfer(address operator, uint256 tkId) public view returns (bool)
```

### allowTransferOf

```solidity
function allowTransferOf(address operator, uint256 tkId) public virtual
```

### _preventUnauthorizedTransferOf

```solidity
function _preventUnauthorizedTransferOf(uint256[] ids) internal view
```

## OwnableExclusiveTransferrableTokens

### __OwnableExclusiveTransferrableTokens_init

```solidity
function __OwnableExclusiveTransferrableTokens_init() internal
```

### allowTransferOf

```solidity
function allowTransferOf(address operator, uint256 tkId) public
```

### _checkOwnerLater

```solidity
function _checkOwnerLater() internal view
```

Check ownership of the actual contract after .transferOwnership have been operated
At the stage of execution of this, the owner of the contract should be the appropriate sender
(EOA for direct calls, smart account for contract calls)

## AccessControlExclusiveTransferrableTokens

### __AccessControlExclusiveTransferrableTokens_init

```solidity
function __AccessControlExclusiveTransferrableTokens_init() internal
```

### allowTransferOf

```solidity
function allowTransferOf(address operator, uint256 tkId) public
```

