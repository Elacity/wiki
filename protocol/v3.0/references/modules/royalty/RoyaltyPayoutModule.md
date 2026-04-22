# RoyaltyPayoutModule
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/52ca0e7824ef5fab5ebe0a131f7c6e6dd330de09/contracts/modules/royalty/RoyaltyPayoutModule.sol)

**Inherits:**
[RoyaltyReentrancyGuard](/contracts/modules/royalty/RoyaltyReentrancyGuard.md), Initializable, [PaymentModule](/contracts/modules/payment/PaymentModule.md)

**Title:**
RoyaltyPayoutModule

Handles payout routing for royalty receivers using the configured payment processor.

Integrates with `PaymentModule` and applies transient-storage reentrancy protection on payouts.


## Functions
### __RoyaltyPayoutModule_init

Initializes the RoyaltyPayoutModule

Core initialization method that must be called by inheriting contracts.

**Note:**
docs-ignore: true


```solidity
function __RoyaltyPayoutModule_init() internal onlyInitializing;
```

### _payRoyalties

Executes the payout of royalties to an array of recipients.

Pays each receiver listed in the `rs` array via `_payAmount`. It can optionally defer payments via `deferPaymentIfQualified`.


```solidity
function _payRoyalties(
    IPaymentProcessor payer,
    address from,
    IERC2981Enhanced.RoyaltyInfo[] memory rs,
    address _payToken
) internal nonReentrant;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`payer`|`IPaymentProcessor`|The `IPaymentProcessor` implementation to use for routing funds.|
|`from`|`address`|The address source of funds.|
|`rs`|`IERC2981Enhanced.RoyaltyInfo[]`|An array of `IERC2981Enhanced.RoyaltyInfo` detailing receiver splits.|
|`_payToken`|`address`|Valid ERC20 address or zero-address for native (ETH) token.|


