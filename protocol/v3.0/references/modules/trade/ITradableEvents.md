# ITradableEvents
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/a429f79c38ae4f5221da86eca62d9868f0a5a7fd/contracts/modules/trade/ITradableEvents.sol)

**Title:**
ITradableEvents

Provides event definitions for tradable flow.


## Events
### ItemListed
Emitted when an item is listed


```solidity
event ItemListed(
    address indexed seller,
    address indexed op,
    uint256 indexed tkId,
    uint256 quantity,
    uint256 pricePerToken,
    address payToken
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|The address of the seller|
|`op`|`address`|The address of the operative|
|`tkId`|`uint256`|The token id of the token|
|`quantity`|`uint256`|The quantity of the token|
|`pricePerToken`|`uint256`|The price per token|
|`payToken`|`address`|The address of the token to pay with|

### ItemSold
Emitted when an item is sold


```solidity
event ItemSold(
    address seller,
    address indexed buyer,
    address indexed op,
    uint256 indexed tkId,
    address payToken,
    uint256 unitPrice,
    uint256 price
);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|The address of the seller|
|`buyer`|`address`|The address of the buyer|
|`op`|`address`|The address of the operative|
|`tkId`|`uint256`|The token id of the token|
|`payToken`|`address`|The address of the token to pay with|
|`unitPrice`|`uint256`|The price per token|
|`price`|`uint256`|The total price|

### ItemUnlisted
Emitted when an item is unlisted


```solidity
event ItemUnlisted(address indexed seller, address indexed op, uint256 indexed tkId, uint256 quantity);
```

**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`seller`|`address`|The address of the seller|
|`op`|`address`|The address of the operative|
|`tkId`|`uint256`|The token id of the token|
|`quantity`|`uint256`|The quantity of the token|

