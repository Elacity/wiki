# ITokenOfferable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/trade/ITokenOfferable.sol)

**Title:**
ITokenOfferable

Provides methods and structs to manage token offer flow.


## Functions
### createOffer

Creates an offer for a token


```solidity
function createOffer(
    address _contract,
    uint256 tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address payToken
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|
|`payToken`|`address`|The address of the token to pay with|


### acceptOffer

Accepts an offer for a token


```solidity
function acceptOffer(address from, address _contract, uint256 tokenId, uint256 _quantity) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address of the offerer|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|
|`_quantity`|`uint256`|The quantity of the token|


### cancelOffer

Cancels an offer for a token


```solidity
function cancelOffer(address _contract, uint256 tokenId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|


## Events
### OfferSettled
Emitted when an offer is settled


```solidity
event OfferSettled(
    address indexed from,
    address indexed _contract,
    uint256 indexed tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address payToken
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address of the offerer|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|
|`payToken`|`address`|The address of the token to pay with|

### OfferCanceled
Emitted when an offer is canceled


```solidity
event OfferCanceled(address indexed from, address indexed _contract, uint256 indexed tokenId);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|The address of the offerer|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|

### OfferAccepted
Emitted when an offer is accepted


```solidity
event OfferAccepted(
    address by,
    address indexed from,
    address indexed _contract,
    uint256 indexed tokenId,
    uint256 _quantity,
    uint256 _pricePerToken,
    address payToken
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`by`|`address`|The address of the offer acceptor|
|`from`|`address`|The address of the offerer|
|`_contract`|`address`|The address of the contract|
|`tokenId`|`uint256`|The token id of the token|
|`_quantity`|`uint256`|The quantity of the token|
|`_pricePerToken`|`uint256`|The price per token|
|`payToken`|`address`|The address of the token to pay with|

