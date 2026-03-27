# OperativeBuyableSellable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/674fb60a18e2aa14b7080f0f43e11002723bd5b3/contracts/operative/kind/OperativeBuyableSellable.sol)

**Inherits:**
[OperativePrimitive](/contracts/operative/kind/OperativePrimitive.md), [IResellable](/contracts/modules/trade/IResellable.md)

**Title:**
OperativeBuyableSellable

"Buy, play, and resell" operative (type 2).
Extends the buy-play model by allowing access-token holders to resell their tokens
on the secondary market. When an access token is transferred, distribution rights
are automatically granted to the new owner and revoked from the previous one
(if they no longer hold any access tokens).

Deployed behind a beacon proxy via `OperativeBuyableSellableFactory`.
The `resellerCut` (basis points) is capped by `MAX_RESELLER_CUT` to prevent
full-value extraction on secondary sales.


## State Variables
### OP_TYPE
Operative-type discriminator (`2` = buy-play-sell).


```solidity
uint16 public constant OP_TYPE = 2
```


### MAX_RESELLER_CUT
Protocol ceiling for reseller cut in basis points (950 = 95%).


```solidity
uint16 public constant MAX_RESELLER_CUT = 950
```


### resellerCut
Percentage of resale price retained by the reseller, in basis points.


```solidity
uint16 public resellerCut = 0
```


### NAME

```solidity
string internal constant NAME = "Op-2 (Can-Sell)"
```


## Functions
### onlyDistributor

Restricts the decorated function to holders of `DISTRIBUTION_RIGHT` tokens.


```solidity
modifier onlyDistributor(address from) ;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address to verify.|


### _onlyDistributor

Restricts the decorated function to holders of `DISTRIBUTION_RIGHT` tokens.


```solidity
function _onlyDistributor(address from) internal view;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address to verify.|


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

### _isDistributor

Checks if the given address holds a `DISTRIBUTION_RIGHT` token.


```solidity
function _isDistributor(address from) internal view returns (bool);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address to verify.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`bool`|`true` if the address holds a `DISTRIBUTION_RIGHT` token, `false` otherwise.|


### setResellerCut

Updates the reseller cut percentage.


```solidity
function setResellerCut(uint16 _resellerCut) public onlyOwner;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_resellerCut`|`uint16`|New cut in basis points (0–950, where 950 = 95 %).|


### _update


```solidity
function _update(address from, address to, uint256[] memory ids, uint256[] memory values)
    internal
    virtual
    override;
```

### supportsInterface

Returns true if this contract implements the interface defined by
`interfaceId`. See the corresponding
https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[ERC section]
to learn more about how these ids are created.
This function call must use less than 30 000 gas.


```solidity
function supportsInterface(bytes4 interfaceId) public view virtual override(OperativePrimitive) returns (bool);
```

## Errors
### UnauthorizedDistrbutorError
Thrown when `from` does not hold a `DISTRIBUTION_RIGHT` token.


```solidity
error UnauthorizedDistrbutorError(address from);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Address that attempted the unauthorised action.|

### ResellerCutOverflow
Thrown when the requested reseller cut exceeds the protocol cap.


```solidity
error ResellerCutOverflow(uint256 value);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`value`|`uint256`|The invalid cut value that was supplied.|

