## RoyaltyPayoutModule

### __RoyaltyPayoutModule_init

```solidity
function __RoyaltyPayoutModule_init() internal
```

### _payRoyalties

```solidity
function _payRoyalties(contract IPaymentProcessor payer, address from, struct IERC2981Enhanced.RoyaltyInfo[] rs, address _payToken) internal
```

## RoyaltyShareModule

### ROYALTY_TOKEN

```solidity
uint256 ROYALTY_TOKEN
```

Token ID of the royalty share

### ShareInput

Input that defines a royalty distribution

```solidity
struct ShareInput {
  address beneficiary;
  uint256 share;
}
```

### __RoyaltyShareModule_init

```solidity
function __RoyaltyShareModule_init(uint256 tokenId) internal
```

_Initializes the RoyaltyShareModule_

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| tokenId | uint256 | The token ID to use for royalty shares |

