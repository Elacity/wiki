# IResellable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/trade/IResellable.sol)

**Title:**
IResellable

Exposes the reseller fee share used during secondary-market sales.


## Functions
### resellerCut

Returns the reseller cut configured by the contract.


```solidity
function resellerCut() external view returns (uint16);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|Basis-point fee share reserved for the reseller.|


