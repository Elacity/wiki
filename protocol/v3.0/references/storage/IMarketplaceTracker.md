# IMarketplaceTracker
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/storage/IMarketplaceTracker.sol)

**Title:**
IMarketplaceTracker

Interface for tracking marketplace listings and offers.

Tracks listings, offers, participants, and marketplace fee settings.


## Functions
### listings

Returns raw listing fields from storage.

keep on track of this issue https://github.com/ethereum/solidity/issues/6337
we need to define Listing here, not in the implementation


```solidity
function listings(address op, uint256 tokenId, address owner) external returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`owner`|`address`|Listing owner/seller.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Quantity, unit price, and payment token address.|
|`<none>`|`uint256`||
|`<none>`|`address`||


### setListing

Sets listing fields for a seller.

**Note:**
docs-ignore: true


```solidity
function setListing(address op, uint256 tokenId, address owner, uint256 qt, uint256 pricePerToken, address payToken)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`owner`|`address`|Listing owner/seller.|
|`qt`|`uint256`|Listed quantity.|
|`pricePerToken`|`uint256`|Unit price per token.|
|`payToken`|`address`|Payment token address.|


### sellersOf

Returns all sellers with listing state for an asset.


```solidity
function sellersOf(address op, uint256 tokenId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Seller address array.|


### getListing

Returns listing details for a specific seller.


```solidity
function getListing(address op, uint256 tokenId, address owner) external view returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`owner`|`address`|Listing owner/seller.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Quantity, unit price, and payment token address.|
|`<none>`|`uint256`||
|`<none>`|`address`||


### offers

Returns raw offer fields from storage.


```solidity
function offers(address op, uint256 tokenId, address owner) external returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`owner`|`address`|Offerer address.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Quantity, unit price, and payment token address.|
|`<none>`|`uint256`||
|`<none>`|`address`||


### setOffer

Sets offer fields for an offerer.

**Note:**
docs-ignore: true


```solidity
function setOffer(address op, uint256 tokenId, address _from, uint256 qt, uint256 pricePerToken, address payToken)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`_from`|`address`|Offerer address.|
|`qt`|`uint256`|Offered quantity.|
|`pricePerToken`|`uint256`|Unit price per token.|
|`payToken`|`address`|Payment token address.|


### offerersOf

Returns all offerers with offer state for an asset.


```solidity
function offerersOf(address op, uint256 tokenId) external view returns (address[] memory);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`address[]`|Offerer address array.|


### getOffer

Returns offer details for a specific offerer.


```solidity
function getOffer(address op, uint256 tokenId, address owner) external view returns (uint256, uint256, address);
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`op`|`address`|Operative contract address.|
|`tokenId`|`uint256`|Asset token id.|
|`owner`|`address`|Offerer address.|

**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint256`|Quantity, unit price, and payment token address.|
|`<none>`|`uint256`||
|`<none>`|`address`||


### taxInformation

Returns marketplace fee settings.


```solidity
function taxInformation() external view returns (uint16, address);
```
**Returns**

|Name|Type|Description|
|----|----|-----------|
|`<none>`|`uint16`|Platform fee in basis points and fee recipient address.|
|`<none>`|`address`||


### setTaxInformation

Sets marketplace fee settings.

**Note:**
docs-ignore: true


```solidity
function setTaxInformation(uint16 _platformFee, address _feeRecipent) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_platformFee`|`uint16`|Platform fee in basis points.|
|`_feeRecipent`|`address`|Fee recipient address.|


