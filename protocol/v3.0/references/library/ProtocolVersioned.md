# ProtocolVersioned
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/library/ProtocolVersioned.sol)

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


