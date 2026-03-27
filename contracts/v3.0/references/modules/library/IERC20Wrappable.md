# IERC20Wrappable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/library/IERC20Wrappable.sol)

**Inherits:**
IERC20

**Title:**
IERC20Wrappable

ERC-20 interface extension for wrapped native-token contracts.


## Functions
### deposit

Wraps native currency into ERC-20 balance.

`msg.value` defines wrapped amount.


```solidity
function deposit() external payable;
```

### withdraw

Unwraps wrapped balance back into native currency.


```solidity
function withdraw(uint256 wad) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`wad`|`uint256`|Amount to unwrap.|


