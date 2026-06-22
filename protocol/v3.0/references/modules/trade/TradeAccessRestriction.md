# TradeAccessRestriction
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/trade/TradeAccessRestriction.sol)

**Inherits:**
ERC165, [ITradeAccessRestriction](/contracts/modules/trade/ITradeAccessRestriction.md)

**Title:**
TradeAccessRestriction

This contract is handling the restriction of trade operations


## Functions
### hasTradeAccess

Check whether an accunt have access to operate trade


```solidity
function hasTradeAccess(address _account, uint256 tkId) public view virtual returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_account`|`address`||
|`tkId`|`uint256`|Target token Id of the tradable token|


### supportsInterface


```solidity
function supportsInterface(bytes4 interfaceId) public view virtual override returns (bool);
```

