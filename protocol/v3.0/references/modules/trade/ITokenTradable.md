# ITokenTradable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/trade/ITokenTradable.sol)

**Inherits:**
[ITradable](/contracts/modules/trade/ITradable.md)

**Title:**
ITokenTradable

Provides methods and structs to manage token trading flow.


## Functions
### sellToken

Sells a token


```solidity
function sellToken(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|
|`_payToken`|`address`|The address of the token to pay with|


### buyToken

Buys a token


```solidity
function buyToken(address seller, address _contract, uint256 tokenId, uint256 _quantity) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|The address of the seller|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|
|`_quantity`|`uint256`|The quantity of the token|


