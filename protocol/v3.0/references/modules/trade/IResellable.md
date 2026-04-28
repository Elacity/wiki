# IResellable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/trade/IResellable.sol)

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


