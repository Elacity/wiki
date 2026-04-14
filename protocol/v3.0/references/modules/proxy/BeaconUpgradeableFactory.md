# BeaconUpgradeableFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/proxy/BeaconUpgradeableFactory.sol)

**Inherits:**
Ownable

**Note:**
docs-ignore: true


## State Variables
### _BEACON

```solidity
UpgradeableBeacon private immutable _BEACON
```


## Functions
### constructor


```solidity
constructor(address _implementation, address initialOwner) Ownable(initialOwner);
```

### beacon


```solidity
function beacon() external view onlyOwner returns (address);
```

### _getBeacon


```solidity
function _getBeacon() internal view returns (address);
```

