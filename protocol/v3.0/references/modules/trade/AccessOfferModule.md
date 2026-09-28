# AccessOfferModule
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/bc1f2ea3fd5d8b703627a7946e7d5fe7fb13f047/contracts/modules/trade/AccessOfferModule.sol)

**Inherits:**
[AccessTradeModule](/contracts/modules/trade/AccessTradeModule.md), [IAccessOfferable](/contracts/modules/trade/IAccessOfferable.md)

Internal creation, settlement and cancellation workflows for access-token buy offers.

Adds no storage or initializer. The gateway supplies shared storage, validates native funding,
resolves ledger wrappers and protects every mutation with its shared buy/offer entry lock.
AccessTradeModule is inherited once to reuse seller policy and payout routing.


## Functions
### _createAccessOffer

Records the caller's offer for a registered operative; native funding is checked by the gateway.

Uses the gateway as custodian. ERC-20 funding remains with the maker until acceptance.


```solidity
function _createAccessOffer(
    IStorage store,
    address op,
    uint256 tokenId,
    uint256 quantity,
    uint256 price,
    address payToken
) internal;
```

### _acceptAccessOffer

Fills the maker's offer with the caller's tokens and routes payment through the access trade module.

The gateway must hold the shared trade lock before calling any offer mutation.


```solidity
function _acceptAccessOffer(
    IStorage store,
    address from,
    address op,
    uint256 tokenId,
    uint256 quantity,
    uint256 expectedPricePerToken,
    address expectedPayToken
) internal;
```

### _cancelAccessOffer

Removes the caller's offer and refunds its remaining native escrow, if any.


```solidity
function _cancelAccessOffer(IStorage store, address op, uint256 tokenId) internal;
```

### _requireAccessOfferOrigin

Validate origin before payment; CentralStorage independently enforces the same boundary on mutation.


```solidity
function _requireAccessOfferOrigin(IStorage store, address op, uint256 tokenId, address from) private view;
```

### _checkAccessOfferToken


```solidity
function _checkAccessOfferToken(uint256 tokenId) private pure;
```

### _checkAccessOfferOperative

Bind the claimed operative back to canonical storage; arbitrary ERC165 responses do not establish registration.


```solidity
function _checkAccessOfferOperative(IStorage store, address op) private view returns (uint16 kind);
```

