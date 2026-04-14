# IResellable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/trade/IResellable.sol)

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


