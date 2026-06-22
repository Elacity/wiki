# ProtocolVersioned
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/library/ProtocolVersioned.sol)

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


