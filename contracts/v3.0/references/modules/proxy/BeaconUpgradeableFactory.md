# BeaconUpgradeableFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/modules/proxy/BeaconUpgradeableFactory.sol)

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

