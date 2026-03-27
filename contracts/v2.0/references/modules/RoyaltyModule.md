## RoyaltyPayoutModule

Handles payout routing for royalty receivers using the configured payment processor.

_Integrates with `PaymentModule` and applies Reentrancy protection on payouts._

### _payRoyalties

```solidity
function _payRoyalties(contract IPaymentProcessor payer, address from, struct IERC2981Enhanced.RoyaltyInfo[] rs, address _payToken) internal
```

Executes the payout of royalties to an array of recipients.

_Pays each receiver listed in the `rs` array via `_payAmount`. It can optionally defer payments via `deferPaymentIfQualified`._

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |
| payer | contract IPaymentProcessor | The `IPaymentProcessor` implementation to use for routing funds. |
| from | address | The address source of funds. |
| rs | struct IERC2981Enhanced.RoyaltyInfo[] | An array of `IERC2981Enhanced.RoyaltyInfo` detailing receiver splits. |
| _payToken | address | Valid ERC20 address or zero-address for native (ETH) token. |

## RoyaltyShareModule

Defines common structs and state variables to manage fractional royalty shares.

_Intended to be inherited by contracts requiring royalty distribution shares to be mapped by tokens._

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

