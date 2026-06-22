# IAccessTradable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/modules/trade/IAccessTradable.sol)

**Inherits:**
[ITradable](/contracts/modules/trade/ITradable.md)

**Title:**
IAccessTradable

Provides methods and structs to manage access trading flow.


## Functions
### sellAccess

Put a digital asset on sale, the asset here is defined by its location in the ledger context


```solidity
function sellAccess(address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken, address _payToken)
    external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`ledger`|`address`|The address of the ledger|
|`tokenId`|`uint256`|The id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|
|`_payToken`|`address`|The address of the token to pay with|


### sellAccessOnBehalf

Put a digital asset on sale on behalf of a user through a non-EOA address, the asset here is defined by its location in the ledger context
the exection could require ERC1155 approval from the seller to succeed


```solidity
function sellAccessOnBehalf(
    address seller,
    address ledger,
    uint256 tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address _payToken
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|The address of the seller|
|`ledger`|`address`|The address of the ledger|
|`tokenId`|`uint256`|The id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|
|`_payToken`|`address`|The address of the token to pay with|


### buyAccess

This moethod requires ERC1155 approval from the buyer in prior to the execution

Buy a digital asset, the asset here is defined by its location in the ledger context.
The amount that should be passed into `msg.value` should fulfill the operation and should not be less than _quantity * _pricePerToken


```solidity
function buyAccess(address seller, address ledger, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken)
    external
    payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|The address of the seller|
|`ledger`|`address`|The address of the ledger|
|`tokenId`|`uint256`|The id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|


### buyAccess

This moethod requires ERC1155 approval from the buyer in prior to the execution

Buy a digital asset, the asset here is defined by its location in the ledger context.
Payment token here should comply with ERC20 standard
The amount that should be passed into `msg.value` should fulfill the operation and should not be less than _quantity * _pricePerToken


```solidity
function buyAccess(
    address seller,
    address ledger,
    uint256 tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address _payToken
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|The address of the seller|
|`ledger`|`address`|The address of the ledger|
|`tokenId`|`uint256`|The id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|
|`_payToken`|`address`|The address of the token to pay with, should be ERC20 compliant|


