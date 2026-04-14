# IERC20Wrappable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/library/IERC20Wrappable.sol)

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


