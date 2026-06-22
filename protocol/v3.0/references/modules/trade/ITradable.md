# ITradable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/trade/ITradable.sol)

**Inherits:**
[ITradableErrors](/contracts/modules/trade/ITradableErrors.md), [ITradableEvents](/contracts/modules/trade/ITradableEvents.md)

**Title:**
ITradable

Provides methods and structs to manage tradable flow.


## Functions
### withdrawListing

Allows a seller to withdraw token from listing. the quantity should be less than or
equal to the quantity listed


```solidity
function withdrawListing(address op, uint256 tokenId, uint256 quantity) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|The address of the target operative contract|
|`tokenId`|`uint256`|The id of the token|
|`quantity`|`uint256`|The quantity to withdraw|


### listings

Get the listing details of a digital asset, the asset here is defined directly
from its location within the Operative contract


```solidity
function listings(address op, uint256 tokenId, address seller) external view returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|The address of the operative|
|`tokenId`|`uint256`|The id of the token|
|`seller`|`address`|The address of the seller|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|The quantity, price per token and the payment token address|
|`<none>`|`uint256`||
|`<none>`|`address`||


### sellersOf

Get the sellers of a digital asset, the asset here is defined within the operative contract context


```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|The address of the operative|
|`tokenId`|`uint256`|The id of the token|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|The sellers of the token|


