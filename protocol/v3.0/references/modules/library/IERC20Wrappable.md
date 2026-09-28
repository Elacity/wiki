# IERC20Wrappable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/library/IERC20Wrappable.sol)

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


