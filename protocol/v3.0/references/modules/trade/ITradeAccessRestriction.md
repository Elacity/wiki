# ITradeAccessRestriction
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/trade/ITradeAccessRestriction.sol)

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


