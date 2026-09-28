# IAccessOfferable
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/trade/IAccessOfferable.sol)

Buy offers for a registered operative's ACCESS_TOKEN (id 1), without expiry.


## Functions
### createOffer

Creates an ERC-20 offer; funds remain with the maker until acceptance.

No expiry or replacement. Approve Authority for direct fees and the operative processor for routed payouts.


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
|`_contract`|`address`|Canonical registered operative, not the channel address.|
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1), not the channel media id.|
|`_quantity`|`uint256`|Positive number of pre-minted tokens requested.|
|`_pricePerToken`|`uint256`|Positive amount in payment-token smallest units.|
|`payToken`|`address`|Nonzero ERC-20 address; the native sentinel is rejected.|


### createOffer

Creates an exactly funded native offer held by this gateway.

msg.value must equal quantity times unit price. Funds stay escrowed until fill or maker cancellation.


```solidity
function createOffer(address _contract, uint256 tokenId, uint256 _quantity, uint256 _pricePerToken) external payable;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|Canonical registered operative.|
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1).|
|`_quantity`|`uint256`|Positive number of tokens requested.|
|`_pricePerToken`|`uint256`|Positive native price per token in wei.|


### acceptOffer

Accepts current terms only if price and payment token match the seller's expectations.

Caller is seller; maker pays and receives tokens. Current OP1 owner only; eligible OP2 holders may resell.


```solidity
function acceptOffer(
    address from,
    address _contract,
    uint256 tokenId,
    uint256 _quantity,
    uint256 expectedPricePerToken,
    address expectedPayToken
) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`from`|`address`|Offer maker and recipient of the tokens.|
|`_contract`|`address`|Registered operative with the offer.|
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1).|
|`_quantity`|`uint256`|Positive fill quantity no greater than remaining quantity.|
|`expectedPricePerToken`|`uint256`|Exact agreed unit price; rejects changed terms on recreation, but identical terms may still fill a recreated offer.|
|`expectedPayToken`|`address`|Exact payment asset agreed to; zero means native currency.|


### cancelOffer

Cancels the maker's own offer and atomically refunds any remaining native escrow.

No current operative eligibility check; a rejecting refund receiver restores the whole offer.


```solidity
function cancelOffer(address _contract, uint256 tokenId) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`_contract`|`address`|Operative recorded in the offer, even if current registry resolution has changed.|
|`tokenId`|`uint256`|Must equal ACCESS_TOKEN (1).|


## Events
### OfferSettled
Creates an offer; `_contract` is the operative, tokenId is 1, and `from` is the maker.


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

### OfferCanceled
Terminal maker cancellation, emitted before a native refund callback.


```solidity
event OfferCanceled(address indexed from, address indexed _contract, uint256 indexed tokenId);
```

### OfferAccepted
`by` sells `_quantity` tokens into `from`'s offer at the stored unit price.


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

## Errors
### InvalidAccessOperative
The operative is not registered, does not expose the required interface, or has an unsupported type.


```solidity
error InvalidAccessOperative(address operative);
```

### AccessOfferNotFound
No active offer exists for this operative and maker.


```solidity
error AccessOfferNotFound(address operative, address offerer);
```

### SelfOfferAcceptance
A maker cannot sell tokens into their own offer.


```solidity
error SelfOfferAcceptance();
```

