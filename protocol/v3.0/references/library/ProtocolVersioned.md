# ProtocolVersioned
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/library/ProtocolVersioned.sol)

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


