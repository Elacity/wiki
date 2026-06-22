# IResellable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/trade/IResellable.sol)

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


