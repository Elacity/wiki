## ChannelFoundationFactory

Abstract base for all channel factories. Provides shared helpers for creating
a payment processor and authorizing token transfers on newly deployed channel proxies.

_Immutable references to the `IPaymentProcessorFactory` and `TradeGateway` are
set once at construction time and shared by all concrete factory implementations._

### paymentProcessorFactory

```solidity
contract IPaymentProcessorFactory paymentProcessorFactory
```

### tradeGateway

```solidity
address tradeGateway
```

### constructor

```solidity
constructor(contract IPaymentProcessorFactory _ppf, address _tg) internal
```

### _setPaymentProcessor

```solidity
function _setPaymentProcessor(address channelAddress) internal
```

### _allowTransferOf

```solidity
function _allowTransferOf(address channel, address operator, uint256 tkId) internal
```

