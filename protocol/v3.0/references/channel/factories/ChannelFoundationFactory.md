# ChannelFoundationFactory
[Git Source](https://github.com/Elacity/v3-drm-protocol/blob/9e5d1dcd32c5761e2bd56d37138c1de7aac83865/contracts/channel/factories/ChannelFoundationFactory.sol)

**Title:**
ChannelFoundationFactory

Abstract base for all channel factories. Provides shared helpers for creating
a payment processor and authorizing token transfers on newly deployed channel proxies.

Immutable references to the `IPaymentProcessorFactory` and `TradeGateway` are
set once at construction time and shared by all concrete factory implementations.


## State Variables
### paymentProcessorFactory

```solidity
IPaymentProcessorFactory internal immutable paymentProcessorFactory
```


### tradeGateway

```solidity
address internal immutable tradeGateway
```


## Functions
### constructor


```solidity
constructor(IPaymentProcessorFactory _ppf, address _tg) ;
```

### _setPaymentProcessor


```solidity
function _setPaymentProcessor(address channelAddress) internal returns (address payProc);
```

### _allowTransferOf


```solidity
function _allowTransferOf(address channel, address operator, uint256 tkId) internal;
```

