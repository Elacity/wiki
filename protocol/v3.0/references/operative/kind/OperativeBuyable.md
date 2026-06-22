# OperativeBuyable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/operative/kind/OperativeBuyable.sol)

**Inherits:**
[OperativePrimitive](/contracts/operative/kind/OperativePrimitive.md)

**Title:**
OperativeBuyable

"Buy once, play always" operative (type 1).
Purchasers receive an `ACCESS_TOKEN` that grants permanent playback rights.
Only the original creator (or a designated address) holds a `DISTRIBUTION_RIGHT`
token, making them the sole party authorised to sell new access tokens.

Deployed behind a beacon proxy via `OperativeBuyableFactory`.


## State Variables
### OP_TYPE
Operative-type discriminator (`1` = buy-play).


```solidity
uint16 public constant OP_TYPE = 1
```


### NAME

```solidity
string internal constant NAME = "Op-1 (Buy-Play)"
```


## Functions
### constructor

**Notes:**
- oz-upgrades-unsafe-allow: constructor

- docs-ignore: true


```solidity
constructor() OperativePrimitive();
```

### name


```solidity
function name() public pure returns (string memory);
```

### initialize

**Note:**
docs-ignore: true


```solidity
function initialize(IStorage _dataStorage, bytes16 _contentId, string calldata baseURI)
    public
    virtual
    override
    initializer;
```

### _update


```solidity
function _update(address from, address to, uint256[] memory ids, uint256[] memory values)
    internal
    virtual
    override;
```

