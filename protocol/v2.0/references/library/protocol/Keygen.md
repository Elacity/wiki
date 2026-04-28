## Keygen

### generatePrivKey

```solidity
function generatePrivKey(uint8 factor) public view returns (bytes32)
```

Generate a new private key

_The factor here is to elevate timestamp in power to minimize producing same key
for same user in short amount of time. More is the factor, less will be the risk to have same key
0 is an acceptable value
8 is the maximum_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| factor | uint8 | uint8 - power elevation factor |

