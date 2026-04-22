# ExclusiveTransferrableTokens
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/library/ExclusiveTransferrableTokens.sol)

**Inherits:**
[IExclusiveTransferrableTokens](/contracts/modules/library/IExclusiveTransferrableTokens.md)

**Title:**
ExclusiveTransferrableTokens

Abstract contract that implements the `IExclusiveTransferrableTokens` interface


## State Variables
### EXCLUSIVE_TRANSFERRABLE_TOKENS_STORAGE_LOCATION

```solidity
bytes32 private constant EXCLUSIVE_TRANSFERRABLE_TOKENS_STORAGE_LOCATION =
    0x4ae4bc2357e219c56e01ee7e39d25e49f77a0bd8bf28b03d5004638d2b80ea00
```


## Functions
### _getExclusiveTransferrableTokensStorage


```solidity
function _getExclusiveTransferrableTokensStorage()
    private
    pure
    returns (ExclusiveTransferrableTokensStorage storage $);
```

### _slotFor


```solidity
function _slotFor(address operator, uint256 tkId) private pure returns (bytes32 slotId);
```

### allowedTransfer


```solidity
function allowedTransfer(address operator, uint256 tkId) public view returns (bool);
```

### _allowTransferOf


```solidity
function _allowTransferOf(address operator, uint256 tkId) internal virtual;
```

### _isExclusiveToken


```solidity
function _isExclusiveToken(uint256 tkId) private view returns (bool);
```

### _preventUnauthorizedTransferOf


```solidity
function _preventUnauthorizedTransferOf(uint256[] memory ids) internal view;
```

## Events
### OperatorAllowed
Emitted when an operator is allowed to transfer a token.


```solidity
event OperatorAllowed(address indexed initiator, address indexed operator, uint256 indexed tokenId);
```

## Errors
### UnauthorizedTransfer
Thrown when an unauthorized transfer is attempted.


```solidity
error UnauthorizedTransfer();
```

## Structs
### ExclusiveTransferrableTokensStorage
**Note:**
storage-location: erc7201:elacity.drm.storage.ExclusiveTransferrableTokens


```solidity
struct ExclusiveTransferrableTokensStorage {
    mapping(uint256 => bool) exclusiveTokens;
    mapping(bytes32 => bool) allowedTransfer;
}
```

