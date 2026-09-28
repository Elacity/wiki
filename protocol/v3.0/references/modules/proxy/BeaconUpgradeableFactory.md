# BeaconUpgradeableFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/proxy/BeaconUpgradeableFactory.sol)

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

