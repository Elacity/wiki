# ITradeAccessRestriction
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/trade/ITradeAccessRestriction.sol)

**Title:**
ITradeAccessRestriction

This interface defines the requirements for a contract to enable users
interacting with trade gateway contract. For conveniance the contract that implements
it should comply with `ERC-165` as the function is generaly called from outside.


## Functions
### hasTradeAccess

Check whether an accunt have access to operate trade


```solidity
function hasTradeAccess(address account, uint256 tkId) external view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`account`|`address`|Address of the account to check|
|`tkId`|`uint256`|Target token Id of the tradable token|


