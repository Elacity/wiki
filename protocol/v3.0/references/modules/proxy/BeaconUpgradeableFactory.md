# BeaconUpgradeableFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/proxy/BeaconUpgradeableFactory.sol)

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

