# ProtocolVersioned
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/library/ProtocolVersioned.sol)

**Title:**
ProtocolVersioned

Shared protocol-version surface for ecosystem contracts.


## Functions
### protocolVersion

Returns the protocol major/minor version derived from `Ecosystem.VERSION`.


```solidity
function protocolVersion() public pure virtual returns (string memory);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`string`|Version string in `major.minor` format (for example `3.0`).|


